# `matlab`画函数方法

直接计算法：在运算符号前加点`.`

```matlab
clear
t = linspace(0,20,501);
f1 = sin(t).*(1-t.^2);
plot(t,f1,'-');
```

使用`subs`函数：提前规定一个函数，利用`subs`函数进行赋值

```matlab
tic		%启动一个计时器
clear	%清除工作空间
syms x	%声明一个符号变量x
f = sin(x)*(1-x^2);		%定义一个函数
t = linspace(0,20,501);	%从0到20，含有501个点，定义了一个均匀的时间或空间网格
f1 = subs(f,x,t);	%符号函数f，符号变量x替换成数值向量t
plot(t,f1,'-');		%绘制一个二位图像，用‘-’来连接
toc		%计时结束，显示代码执行的时间
```



函数

```matlab
title(['直接计算法用时',num2str(t2),'s'])
%{
	title取标题
	[字符串数组]
	num2str()将数值转化为字符串形式
	s：字符串常量。
}%
```

具体实例

```matlab
f = @(x) exp(x)./(1 + exp(2*x)).^1.5	%定义函数
fplot(f,[-2,2]);	%绘制，x轴范围为-2到2
xlabel('x');	%x轴标签
ylabel('f(x)');
title('函数')
grid on;	%开启网格
```

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402010920620.png" style="zoom:67%;" />



```matlab
% $$e ^ { 2 x }(1+e^{2x})^{\frac{3}{2} } $$
f = @(x) exp(2*x).*(1 + exp(2*x)).^(3/2);
x_range = [-2,2];
fplot(f,x_range);
xlabel('x');
ylabel('f(x)')
grid on;
```

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402010937392.png" style="zoom:67%;" />

```matlab
f = @(x) (x-1)./(1-exp(x));
x_range = [-2,2];
fplot(f,x_range);
xlabel('x');
ylabel('f(x)')
grid on;
```

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402010951457.png" style="zoom:67%;" />

```matlab
f = @(x) log(exp(x) - 1./x);
x_range = [-20,-0.01];	%从-2到-0.01
fplot(f,x_range);
hold on;	%保持当前图像
x_range = [0.01,20]
fplot(f,x_range);
hold off;

xlabel('x');
ylabel('f(x)')
grid on;
```

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402011009643.png)

```matlab
f = @(x) log(exp(1) - 1./x);
x_range = [-20,-0.01];	%从-20到-0.01
fplot(f,x_range);
hold on;	%保持当前图像
x_range = [0.01,20]
fplot(f,x_range);
hold off;

xlabel('x');
ylabel('f(x)')
grid on;
```

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402011015229.png)



```matlab
f = @(x) (1 + x).^3 ./ (1-x).^2;
x_range = [-2,0.99];	%从-2到0
fplot(f,x_range);
hold on;	%保持当前图像
x_range = [1.01,2]
fplot(f,x_range);
hold off;

xlabel('x');
ylabel('f(x)')
grid on;
```

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402011036827.png)

