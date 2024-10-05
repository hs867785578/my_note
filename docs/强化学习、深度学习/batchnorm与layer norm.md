batch norm：b,c,h,w的输入，均值的个数为c，求均值的分母为bhw

![](images/batchnorm与layer%20norm_image_1.png)

layer norm：b,c,h,w -> b,hw,c，均值的个数为bhw，求均值的分母为c
