# 408Note
考研计算机408复习笔记—计算机组成原理

一、数据的表示和运算

1.定点数的编码表示

(1).原码

  最高位为符号位，0为正，1为负。例如：(机器字长为8位) x1=+0.1101,[x1]原=0.1101000; x2=-0.1101,[x2]原=1.1101000; x3=+1110,[x3]原=0,0001110;x2=-1110,[x2]原=1,0001110。

  若字长为n+1，则原码小数的表示范围为$-(1-2^{-n})\leq x\leq 1-2^{-n}。原码整数的表示范围为。
  原码的缺点：符号位不能参与运算。

(2).补码

  原码加减运算比较复杂，补码的加减运算统一采用加法操作实现。
  正数的原码、反码、补码表示不变。
  负数的原码和反码互化：符号位不变，数值位取反。
  负数的反码转化为补码：末位+1。
  原码和补码互化快速方法：从右往左找到第一个“1”，该位左边所有数值为全部按位取反，符号位保持不变，该位及其右边的位不变。

  计算机硬件做补码的加法：从最低为开始，按位相加(符号位参与运算)，并向更高位进位。
  计算机硬件做补码的减法运算：[A]补-[B]补 => [A]补+[-B]补。(补码求其负值：全部位按位取反，末位+1）。

  若字长为n+1，则补码小数的表示范围为(比原码多表示-1)。补码整数的表示范围为(比原码多表示)。
