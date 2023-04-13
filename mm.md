# 虚拟地址
安全，隔离  
虚拟地址连续 != 物理地址连续  
Swap

# 页表
CR3寄存器: 指向页表基地址，告诉硬件从哪里开始查页表项目.  
虚拟地址 --> 物理地址 假设用48位虚拟地址，用数组做映射 2^(48-12)*8 bytes = 512GB  
按需加载 --> 树 --> (访存次数, 充分利用一个页的空间) --> 深度不能太深 --> 多叉树 
![pagatable](https://img-blog.csdnimg.cn/img_convert/054250bdc15b16c1980bfe37eb24a7b7.png)
4096 = 8 * 512 = 8 * (2^9)
# 内存虚拟化
GVA --> GPA --> HPA  

Guest内核无法得到HOST的物理地址（内存隔离） 
GPA --> HPA 只能借助Host的能力才能完成  
MMU: GVA --> HPA 才能正常访存  

最土的方法: CR3永远指向一个页表基地址，截获 TLB 以及 cr3 寄存器操作。  
TLB: VA --> PA 的快速查找表（类似于hash表），页表scan需要访问内存，对CPU来说是一种慢行为，且查找层数越多，性能越差. 切换页表或者页表项修改了，都会flush tlb。    
Pagefault到Host看看guest有没有准备好 GVA --> GPA，没准备好让guest准备好，guest准备好了，但是真正硬件看的那个页表没有准备，又会fault出来。然后host将对应的表准备好。  
TLB flush部分数据，说明guest内部某个页表项换了，host根据相关内容做对应修改。就是 GVA --> GPA_old ==> GVA --> GPA_new，将原来GVA对应的HPA_old改成HPA_new就行了。 

# 影子页表
GVA --> HPA  
在HOST维护GUEST内部进程的页表  
截获对GUEST内的页表的操作 --> 所有写操作都会vmexit到HOST处理，准备好对应真正用的页表。
![shadow_pgtable](https://rayanfam.com/assets/images/shadow-page-tables-1.png)

# PVMMU
半虚拟化, 即guest内核知道自己在虚拟化环境中，读/写页表的函数中直接进行 GPA <--> HPA 的相互转换。即让硬件MMU看到的是 GVA --> HPA 的映射关系。  
GPA <--> HPA的转换表可以陷出让VMM帮自己做。

# ept硬件加速
GVA --> GPA --> HPA (通过硬件完成)
![ept](https://img2020.cnblogs.com/blog/774036/202104/774036-20210415150835736-1973177805.png).  
通过EPT fault将 GPA --> HPA的页表准备好。

# 通过GPA找HPA
GUEST里的一段物理内存和VMM程序的一段虚拟地址内存对应的  
GPA对应的Guest PFN --> Host VFN --> (get_user_pages: vma --> pages) Host PFN
