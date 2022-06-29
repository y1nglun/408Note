# 408Note
考研计算机408复习笔记—计算机组成原理

一、数据的表示和运算

1.定点数的编码表示

(1).原码

    最高位为符号位，0为正，1为负。
  
    例如：(机器字长为8位) x1=+0.1101,[x1]原=0.1101000;
    x2=-0.1101,[x2]原=1.1101000;
    x3=+1110,[x3]原=0,0001110;x2=-1110,[x2]原=1,0001110。

  原码的真值0有两种表达方式，分别是[+0原]和[-0原]。
  
   若字长为n+1，则原码小数的表示范围为$-(1-2^{-n})\leq x\leq 1-2^{-n}$。
   原码整数的表示范围为$-(2^{n}-1)\leq x\leq 2^{n}-1$。
  
  原码的缺点：符号位不能参与运算。

(2).补码

  原码加减运算比较复杂，补码的加减运算统一采用加法操作实现。
  
  补码的真值0只有一种表达方式。
  
  正数的原码、反码、补码表示不变。
  
  负数的原码和反码互化：符号位不变，数值位取反。
  
  负数的反码转化为补码：末位+1。
  
  原码和补码互化快速方法：从右往左找到第一个“1”，该位左边所有数值为全部按位取反，符号位保持不变，该位及其右边的位不变。

  计算机硬件做补码的加法：从最低为开始，按位相加(符号位参与运算)，并向更高位进位。
  
  计算机硬件做补码的减法运算：[A]补-[B]补 => [A]补+[-B]补。(补码求其负值：全部位按位取反，末位+1）。

    若字长为n+1，则补码小数的表示范围为$-1\leq x\leq 1-2^{-n}$(比原码多表示-1)。
    补码整数的表示范围为$-2^{n}\leq x\leq 2^{n}-1$(比原码多表示)。
  
(3).移码

  在补码的基础上，符号位取反。[注]：移码只能用于表示整数。
  
    移码的表示范围：$-2^{n}\leq x\leq 2^{n}-1$。
  
  
  
二.运算方法和运算电路

  1.一位全加器
  
   全加器(FA)是最基本的加法单元，有加数Ai、加数Bi和低位传来的进位Ci-1共三个输入，有本位和Si与向高位的进位Ci共两个输出。
    
   全加器的逻辑表达式：
    
   和表达式：$Si=Ai\oplus Bi\oplus Ci-1$
   (Ai，Bi，Ci中有奇数个1时，Si=1；否则Si=0)。
    
   进位表达式：
   
   $Ci=AiBi+\left (  Ai\oplus Bi\right )Ci-1$
   
 2.串行进位加法器
   
   把n个全加器相连得到n位加法器。最长运算时间主要是由进位信号的传递时间决定的，位数越多延迟时间就越长。
   
 3.并行进位加法器
 
   令Gi=AiBi，$Pi=Ai\oplus Bi$。
   
          Ci=Gi+PiPi-1(Gi=1或PiCi-1=1时，Ci=1)
   
   C1=G1+P1C0
   
   C2=G2+P2C1=G2+P2G1+P2P1C0
   
   C3=G3+P3C2=G3+P3G2+P3P2G1+P3P2P1C0
   
   一般四个一组实现并行计算进位，该逻辑表达式的电路称为先行进位(也称超前进位)部件，简称CLA部件。
   
   这种进位方式是快速的，与位数无关。但随着加法器位数的增加，Ci的逻辑表达式会变得越来越长，这会使电路结构变得很复杂。
   
 4.补码加减运算电路
 
  计算机有符号整数与无符号整数进行加减运算使用的是相同的补码加减运算部件，但判断溢出时有符号整数与无符号整数完全不同。
  
  标志位：
  
  OF(Overflow Flag)溢出标志：仅在有符号数的加减运算中有意义，溢出时为0，否则置1。
   
   硬件的计算方法：$OF=最高位产生的进位\oplus 次高位产生的进位$
   
  SF(Sign Flag)符号标志：仅在有符号数的加减运算中有意义，运算结果为正置为0，为负置为1
  
   硬件的计算方法：SF=最高位的本位和
   
  ZF(Zero Flag)零标志：运算结果为0时置为1，否则为0
  
  CF(Carry Flag)进位/借位标志：只对无符号数的加减法有意义，对有符号数的加减法无意义。进位/借位时置1，否则为0
  
   硬件的计算方法：$CF=最高位产生的进位\oplus sub$
   ，sub=1时表示减法，sub=0时表示加法。

 5.移位运算
 
  算数移位：
  
    原码的算数移位——符号位保持不变，仅对数值为进行移位
    
    右移：高位补0，低位舍弃。若舍弃的位=0，则相当于÷2；若舍弃的位≠0，则会丢失精度。
    
    左移：低位补0，高位舍弃。若舍弃的位=0，则相当于×2；若舍弃的为≠0，则会出现严重误差。
    
  逻辑移位：
  
    逻辑右移：高位补0，低位舍弃。
    
    逻辑左移：低位补0，高位舍弃。
    
  循环移位
  
6.定点数的乘除运算

  原码一位乘法：
    
    符号位通过异或确定，数值位由被乘数和乘数的绝对值进行n轮加法、移位。
    
    MQ中最低位为1时，(ACC)+[|x|]原；MQ中最低位为0时，(ACC)+0；
    
    每次移位都是“逻辑右移”，乘数的符号位不参与运算。
    
  补码一位乘法(Booth算法):
    
    符号位、数值位都由被乘数和乘数进行n轮加法、移位，最后再多进行一次加法。
    
    辅助位-MQ中最低位=1时，ACC+[x]补；
    
    辅助位-MQ中最低为=0时，(ACC)+0;
    
    辅助位-MQ中最低为=-1时，(ACC)+[-x]补；
    
    每次移位是“补码的算数右移”，乘数的符号位参与运算。
    
  原码除法(不恢复余数法)：
  
    先用被除数减去除数(|X|-|Y|=|X|+(-|Y|)=|X|+[-|Y|补])，当余数为正时，商上1，余数和商左移一位，再减去除数；
    
    当余数为负时，商上0，余数和商左移一位，再加上除数。
    
    当第n+1步余数为负时，需加上|Y|得到第n+1步正确的余数(余数与被除数同号)。
    
    商符和商值是分开进行的，$Qs=Xs\oplus Ys$。
    
  补码除法(加减交替法)：
  
    1.符号位参与运算，除数与被除数均由补码表示，商和余数也用补码表示
    
    2.若被除数与除数同号，则被除数减去除数；若被除数与除数异号，则被除数加上除数
    
    3.若余数与除数同号，则商上1，余数左移一位减去除数；若余数与除数异号，则商上0，余数左移一位加上除数
    
    4.重复执行第3步操作n此
    
    5.若对商的精度没有特殊要求，则一般采用“末位恒置1”法
    
7.强制类型转换

  无符号数与有符号数：
    
    不改变数据内容，改变解释方式
    
  长整数变短整数：
  
    高位截断，保留低位
    
  短整数变长整数：
  
    符号扩展
    
8.数据的存储和排列

  大小端模式(多字节数据在内存中一定是连续的几个字节)：
  
  大端模式(便于人类阅读)：
  
    先存入高位，后存低位
    
  小端模式(便于机器处理)：
  
    先存低位，后存高位
    
  边界对齐：
  
    每次访存只能读/写一个字。假设存储字长为32位，可按字节、半字和字寻址。对于机器字长为32位的计算机，数据以边界对齐方式存放，
    半字地址一定是2的整数倍，字地址一定是4的整数倍，这样无论所取的数据是字节、半字还是字，均可一次访存取出。所存储的数据不满足上述要求时，
    通过填充空白字节使其符合要求。这样虽然浪费了一些存储空间，但可提高指令和取数的速度。
    
9.浮点数

  浮点数的表示格式：
  
    通常，浮点数表示为：  N=(-1)^s×M×R^E
    
    式中，S取值0或1，用来决定浮点数的符号；M是一个二进制定点小数，称为尾数，一般用定点原码小数表示；E是一个二进制定点整数，称为阶码或指数，
    用移码表示。R是基数(隐含)，可以约定为2、4、16等。可见浮点数由数符、尾数和阶码三部分组成。
    
  浮点数的规格化：
  
    左规：当运算结果的尾数的最高数位不是有效位，需进行左规。左规时，尾数每左移一位、阶码减1，直至尾数变成规格化形式为止。
    
    右规：当运算结果尾数出现溢出(双符号位为01或10时)，需要进行右规。将尾数右移一位、阶码加1.需要右规时，只需进行一次。
    
10.IEEE754

  由浮点数确定真值(阶码不是全0也不是全1):
    
    1.根据“某浮点数”确定数符、阶码、尾数的分布
    2.确定尾数1.M(注意补充最高位隐含的1)
    3.确定阶码的真值=移码-偏置值(可将移码看作无符号数，用无符号数的值减去偏置值)
    4.(-1)^s×M×2^E-偏置值
    
    
