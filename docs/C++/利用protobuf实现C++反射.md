
Protobuf 来定义数据，它有很多好处：

- 有明确的 schema 定义，且支持主流编程语言；在跨团队合作时特别方便，proto 定义文件就是交互协议，不再需要单独的文档
- 支持多种数据格式的序列化和解析：

- 二进制格式：占用空间小，适合网络传输或序列化文件
- Protobuf 的明文格式（ 一般会被称为 pbtxt）：可以通过 msg.DebugString() 生成，配置简单，可用来写配置文件；这个格式可以在调试时打印数据，很方便
- 也支持和 Json、XML 等数据格式相互转换

- 对于 C++ 而言，Protobuf 可以自己高效地管理内存，不需要用户操心
- 很好的前后兼容性，在只使用 optional 的情况下，基本不会遇到兼容性的问题
- **Protobuf 支持反射，可以通过它来实现更好的代码复用**

Protobuf 的反射指的是我们可以在运行时获取 Protobuf Message 的相关信息：

- Message 的字段信息，包括字段名，字段类型等
- 读取或修改 Message 字段的值
- 基于 Message 名称创建或解析对应的 C++ 对象（这个功能本文未使用到）

## 如何使用 Protobuf 反射

反射有三个重要的类：

- [Descriptor](https://link.zhihu.com/?target=https%3A//protobuf.dev/reference/cpp/api-docs/google.protobuf.descriptor/%23Descriptor)：这个类是对应一种 Message 类型的，相同类型 Message 的 Descriptor 是一样的。通过这个类可以获取 Message 有多少个字段，以及获取字段的 FieldDescriptor
- [FieldDescriptor](https://link.zhihu.com/?target=https%3A//protobuf.dev/reference/cpp/api-docs/google.protobuf.descriptor/%23FieldDescriptor)：这个类是用来描述某个具体字段相关的信息的，包括字段的类型、名称、编号等
- [Reflection](https://link.zhihu.com/?target=https%3A//protobuf.dev/reference/cpp/api-docs/google.protobuf.message/%23Reflection)：通过它和 FieldDescriptor，我们就能实现对具体字段的读或者写

下面通过一个具体的例子来说明该怎样使用 Protobuf 反射。

假设我们定义了下面一个 Protobuf Message：

```protobuf
syntax = "proto3";

message Test {
  int32 bar = 1;
  string foo = 2;
}
```

然后我们可以通过如下的代码来遍历一个 Test Message 来打印它所有的字段名，以及对应字段的具体值：

```cpp
#include <cstdio>
#include <iostream>
#include "test.pb.h"
#include <google/protobuf/message.h>

using namespace std;

using google::protobuf::Message;
using google::protobuf::Reflection;
using google::protobuf::Descriptor;
using google::protobuf::FieldDescriptor;

int main(int argc, char *argv[]) {
  Test msg;

  msg.set_bar(42);
  msg.set_foo("I'm hiber");

  Descriptor* msg_descriptor = msg.GetDescriptor();

  for (int i = 0; i < msg_descriptor->field_count(); ++i) {
    FieldDescriptor* fd_descriptor = msg_descriptor->field(i);

    string name = fd_descriptor->name();
    printf("Field name: %s\n", name.c_str());
  }

  Reflection* msg_reflection = msg.GetReflection();
  FieldDescriptor* bar_field = msg_descriptor->FindFieldByName("bar");
  if (bar_field != nullptr) {
    int bar_val = msg_reflection->GetInt32(msg, bar_field);
    printf("Bar value: %d\n", bar_val);
  }

  FieldDescriptor* foo_field = msg_descriptor->FindFieldByName("bar");
  if (foo_field != nullptr) {
    string foo_val = msg_reflection->GetString(msg, foo_field);
    printf("Foo value: %s\n", foo_val.c_str());
  }

  return 0;
}
```

## 基于字段名进行值复制

有时我们需要用两个不同的 Protobuf Message 去表示某个相同的业务实体。例如在我们的广告系统里会有三种不同的 Message 来定义一个广告：

- RetrievalAd：召回模块返回结果里定义的广告，会包括广告在倒排里的信息，例如广告 ID，出价等信息
- Element：广告引擎内部用来表示广告的 Message，除了 RetrievalAd 里的信息，还会包括广告的 pctr、pcvr 等信息
- AdResult：广告引擎返回给外部的信息，会包括广告的素材信息等

这几种结构里会有很多字段是一样的，它们有相同的类型和名称，如果手工进行赋值的话就很麻烦。我们可以通过反射来实现相同名称字段的自动赋值：

```cpp
bool FillMsgWithExistFields(const Message& in, Message* out) {
#define FILL_FIELD_TYPE(cpptype, method) \
  if (type == FieldDescriptor::CPPTYPE_##cpptype) { \
    auto value = in_ref->Get##method(in, in_fd); \
    out_ref->Set##method(out, out_fd, value); \
    continue; \
  }

  if (out == nullptr) {
    return false;
  }
  auto* in_des = in.GetDescriptor();
  auto* out_des = out->GetDescriptor();
  auto* in_ref = in.GetReflection();
  auto* out_ref = out->GetReflection();

  for (int i = 0; i < in_des->field_count(); ++i) {
    auto* in_fd = in_des->field(i);
    const auto& to_field_name = in_fd->options().GetExtension(to_field);
    const FieldDescriptor* out_fd = nullptr;
    const auto& in_name = in_fd->name();
    if (!to_field_name.empty()) {
      out_fd = out_des->FindFieldByName(to_field_name);
    } else {
      out_fd = out_des->FindFieldByName(in_name);
    }

    if (out_fd == nullptr) {
      continue;
    }

    if (out_fd->is_repeated()) {
      AddRepeatField(*in_fd, *out_fd, in, out);
      continue;
    }

    // skip field while repeated type between in and out is not match
    if (in_fd->is_repeated()) {
      continue;
    }

    if (!in_fd->is_repeated() && !in_ref->HasField(in, in_fd)) {
      VLOG(Vlog::DETAIL) << "Field is empty, skip " << in_name;
      continue;
    }

    if (IsPbNumber(in_fd->cpp_type()) && IsPbNumber(out_fd->cpp_type())) {
      SetPbField(out, *out_fd, GetPbMsgInt(in, *in_fd));
      continue;
    }

    if (in_fd->cpp_type() != out_fd->cpp_type()) {
      LOG_EVERY_N(ERROR, 10) << "Field type is different: " << in_name;
      continue;
    }

    auto type = in_fd->cpp_type();
    FILL_FIELD_TYPE(INT32, Int32);
    FILL_FIELD_TYPE(UINT32, UInt32);
    FILL_FIELD_TYPE(INT64, Int64);
    FILL_FIELD_TYPE(UINT64, UInt64);
    FILL_FIELD_TYPE(FLOAT, Float);
    FILL_FIELD_TYPE(DOUBLE, Double);
    FILL_FIELD_TYPE(BOOL, Bool);
    FILL_FIELD_TYPE(STRING, String);

    VLOG(Vlog::AD) << "Not supported message type: " << out_fd->type_name();
  }

  return true;

#undef FILL_FIELD_TYPE
}
```

上面的代码省去例如 AddRepeatField 之类函数的具体实现，他们也是类似的原理，不难实现。

## 基于 Protobuf 反射实现自动规则判定

有时我们需要基于业务要求对一些 Message 进行有效性判断，这些判断通常是确定某几个字段是不是指定的取值，针对每个 Message 都实现判定函数会显得很繁琐，这时我们就可以通过反射和 options 来实现基于配置的规则判定。

假设我们有如下的广告 Message 定义：

```protobuf
message AdGroup {
  uint64 ad_group_id         = 1 ;
  uint64 advertiser_id       = 2 ;

  // reserved fields numbers
  uint32 is_deleted          = 50;
  uint32 operate_status = 51;
} 
```

广告只有在 is_deleted != 0 和 operate_status == 1 的时候才是合法的。

针对这个需求，我们可以先定义两个 FieldOptions：

```protobuf
import "google/protobuf/descriptor.proto";

extend google.protobuf.FieldOptions {
  int64 valid_int_value = 51000;
  bool valid_be_default = 51001;
}
```

给之前的结构加上对应的 option 配置：

```protobuf
message AdGroup {
  uint64 ad_group_id         = 1 ;
  uint64 advertiser_id       = 2 ;

  // reserved fields numbers
  uint32 is_deleted          = 50 [(valid_be_default) = true];
  uint32 operate_status = 51 [(valid_int_value) = 1];
} 
```

然后就可以通过如下的函数来判断一个 Message 是不是有效的：

```cpp
bool IsPbMsgValid(const Message& msg) {
  auto* des = msg.GetDescriptor();
  auto* ref = msg.GetReflection();

  for (int i = 0; i < des->field_count(); ++i) {
    auto* fd_des = des->field(i);

    // Valid the field should be default value
    if (fd_des->options().HasExtension(valid_be_default)) {
      if (ref->HasField(msg, fd_des)) {
        return false;
      }
    }

    if (fd_des->options().HasExtension(valid_int_value)) {
      auto valid_val = fd_des->options().GetExtension(valid_int_value);

      if (GetPbMsgInt(msg, *fd_des) != valid_val) {
        VLOG(Vlog::AD) << "Fail to validate, valid_int_value: field " << fd_des->name()
            << " should be " << valid_val;
        return false;
      }
    }
  }

  return true;
}
```

以后某个 Message 有类似的有效性判定要求时，直接在 proto 里加对应的 option 即可，对于每一种判定规则我们只需要实现一次。

## 总结

Protobuf 反射在我们的线上效果广告系统里广泛使用，在线上峰值 1 万左右的 QPS 下也没有导致额外的性能负载。

通过 Protobuf 反射的使用，能让我们快速支持很多业务需要，例如在支持新的创意样式时，只需要修改 proto 定义即可，不需要额外的代码修改。同时，反射也减少了很多可能的重复代码，降低了维护成本。