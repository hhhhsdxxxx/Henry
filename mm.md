# 虚拟地址
# 页表
虚拟地址-->物理地址 假设用48位虚拟地址，用数组 2^(48-12)*8 bytes  
按需加载 --> 树 --> (访存次数, 充分利用一个页的空间) --> 深度不能太深 --> 多叉树 
![pagatable](https://img-blog.csdnimg.cn/img_convert/054250bdc15b16c1980bfe37eb24a7b7.png)
4096 = 8 * 512 = 8 * (2^9)
# 内存虚拟化
GVA --> GPA --> HPA

# 影子页表
GVA --> HPA

# ept硬件加速
![ept](https://img2020.cnblogs.com/blog/774036/202104/774036-20210415150835736-1973177805.png)
