```python
import torch.nn as nn
import torch
import math

class PositionalEncoding(nn.Module):
    def __init__(self, d_model, dropout=0.1, max_len=5000):
        super(PositionalEncoding, self).__init__()
        self.dropout = nn.Dropout(p=dropout)

        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1)
        _2i = torch.arange(0, d_model, 2)
        item = 1 / (10000 ** (_2i / d_model))
        pe[:, 0::2] = torch.sin(position * item)
        pe[:, 1::2] = torch.cos(position * item)
        pe = pe.unsqueeze(0)
        self.register_buffer('pe', pe)

    def forward(self, x):
        x = x + self.pe[:, :x.size(1)]
        return self.dropout(x)

class MutiHeadSelfAttention(nn.Module):
    def __init__(self, d_model, n_head, dropout=0.1):
        super(MutiHeadSelfAttention, self).__init__()
        assert d_model % n_head == 0
        self.d_k = d_model // n_head
        self.n_head = n_head
        self.w_q = nn.Linear(d_model, d_model)
        self.w_k = nn.Linear(d_model, d_model)
        self.w_v = nn.Linear(d_model, d_model)
        self.w_o = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(p=dropout)

    def forward(self, q, k, v, mask=None):
        batch_size = q.size(0)
        q = self.w_q(q).view(batch_size, -1, self.n_head, self.d_k).transpose(1, 2)
        k = self.w_k(k).view(batch_size, -1, self.n_head, self.d_k).transpose(1, 2)
        v = self.w_v(v).view(batch_size, -1, self.n_head, self.d_k).transpose(1, 2)

        output, attention = self.attention(q, k, v, mask)
        output = output.transpose(1, 2).contiguous().view(batch_size, -1, self.n_head * self.d_k)
        return self.w_o(output)

    def attention(self, q, k, v, mask=None):
        scores = torch.matmul(q, k.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        p_attn = nn.functional.softmax(scores, dim=-1)
        p_attn = self.dropout(p_attn)
        return torch.matmul(p_attn, v), p_attn
    
class TransformerEncoderLayer(nn.Module):
    def __init__(self, d_model, n_head, d_ff, dropout=0.1):
        super(TransformerEncoderLayer, self).__init__()
        self.self_attn = MutiHeadSelfAttention(d_model, n_head, dropout)
        self.pe = PositionalEncoding(d_model, dropout)
        self.ffn = nn.Sequential(
            nn.Linear(d_model, d_ff),
            nn.ReLU(),
            nn.Linear(d_ff, d_model)
        )
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, mask=None):
        x = x + self.pe(x)
        x = x + self.dropout(self.self_attn(x, x, x, mask))
        x = self.norm1(x)
        x = x + self.dropout(self.ffn(x))
        x = self.norm2(x)
        return x

class TransformerEncoder(nn.Module):
    def __init__(self, n_layer, d_model, n_head, d_ff, dropout=0.1):
        super(TransformerEncoder, self).__init__()
        self.layers = nn.ModuleList([TransformerEncoderLayer(d_model, n_head, d_ff, dropout) for _ in range(n_layer)])

    def forward(self, x, mask=None):
        for layer in self.layers:
            x = layer(x, mask)
        return x

class TransformerDecoderLayer(nn.Module):
    def __init__(self, d_model, n_head, d_ff, dropout=0.1):
        super(TransformerDecoderLayer, self).__init__()
        self.self_attn = MutiHeadSelfAttention(d_model, n_head, dropout)
        self.src_attn = MutiHeadSelfAttention(d_model, n_head, dropout)
        self.pe = PositionalEncoding(d_model, dropout)
        self.ffn = nn.Sequential(
            nn.Linear(d_model, d_ff),
            nn.ReLU(),
            nn.Linear(d_ff, d_model)
        )
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.norm3 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, memory, src_mask=None, tgt_mask=None):
        x = x + self.pe(x)
        x = x + self.dropout(self.self_attn(x, x, x, tgt_mask))
        x = self.norm1(x)
        x = x + self.dropout(self.src_attn(x, memory, memory, src_mask))
        x = self.norm2(x)
        x = x + self.dropout(self.ffn(x))
        x = self.norm3(x)
        return x

class TransformerDecoder(nn.Module):
    def __init__(self, n_layer, d_model, n_head, d_ff, dropout=0.1):
        super(TransformerDecoder, self).__init__()
        self.layers = nn.ModuleList([TransformerDecoderLayer(d_model, n_head, d_ff, dropout) for _ in range(n_layer)])

    def forward(self, x, memory, src_mask=None, tgt_mask=None):
        for layer in self.layers:
            x = layer(x, memory, src_mask, tgt_mask)
        return x

class Transformer(nn.Module):
    def __init__(self, n_layer, d_model, n_head, d_ff, dropout=0.1):
        super(Transformer, self).__init__()
        self.encoder = TransformerEncoder(n_layer, d_model, n_head, d_ff, dropout)
        self.decoder = TransformerDecoder(n_layer, d_model, n_head, d_ff, dropout)

    def forward(self, src, tgt, src_mask=None, tgt_mask=None):
        memory = self.encoder(src, src_mask)
        return self.decoder(tgt, memory, src_mask, tgt_mask)

if __name__ == '__main__':
    # 参数设置
    n_layer = 6
    d_model = 256
    n_head = 8
    d_ff = 2048
    dropout = 0.1
    src_len = 10
    tgt_len = 20
    batch_size = 32

    # 创建 Transformer 实例
    model = Transformer(n_layer, d_model, n_head, d_ff, dropout)

    # 生成随机输入数据
    #   示例输入数据
    # src = torch.tensor([
    #     [[1, 2, 3], [4, 5, 6], [0, 0, 0], [7, 8, 9]],
    #     [[0, 0, 0], [1, 1, 1], [2, 2, 2], [0, 0, 0]],
    #     [[1, 2, 3], [4, 5, 6], [0, 0, 0], [7, 8, 9]],
    #     [[0, 0, 0], [1, 1, 1], [2, 2, 2], [0, 0, 0]],
    # ])

    src = torch.randn(batch_size, src_len, d_model)
    tgt = torch.randn(batch_size, tgt_len, d_model)

    # 生成填充掩码
    src_pad_mask = (src != 0).all(dim=-1).unsqueeze(1).unsqueeze(2)  # (batch_size, 1, 1, src_len)
    # tensor([[[[ True,  True, False,  True]]],
    #     [[[False,  True,  True, False]]],
    #     [[[ True,  True, False,  True]]],
    #     [[[False,  True,  True, False]]]])

    tgt_pad_mask = (tgt != 0).all(dim=-1).unsqueeze(1).unsqueeze(2)  # (batch_size, 1, 1, tgt_len)

    # 生成未来掩码
    future_mask = torch.tril(torch.ones((tgt_len, tgt_len)), diagonal=0).bool()
    # tensor([[ True, False, False, False, False],
    #     [ True,  True, False, False, False],
    #     [ True,  True,  True, False, False],
    #     [ True,  True,  True,  True, False],
    #     [ True,  True,  True,  True,  True]])

    # 合并掩码
    future_mask = future_mask.unsqueeze(0).unsqueeze(1)  # (1, 1, tgt_len, tgt_len)
    tgt_mask = tgt_pad_mask & future_mask  # (batch_size, 1, tgt_len, tgt_len)

    # 前向传播
    output = model(src, tgt, src_pad_mask, tgt_mask)

    print(output.size())

    # 检查输出形状
    assert output.shape == (batch_size, tgt_len, d_model), f"Expected output shape {(batch_size, tgt_len, d_model)}, but got {output.shape}"

    print("Test passed!")

```