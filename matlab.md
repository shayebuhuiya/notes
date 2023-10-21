# 二维绘图

## 线图

* plot

  ~~~matlab
  x=0: pi/100: 2*pi;
  y = sin(x);
  plot(x, y);
  
  //同一坐标系用不同格式绘制多种不同格式的线条
  plot(x1, y1, LineSpec1, x2, y2, LineSpec2, ...);
  
  //同上，每次plot后加hold on也可以达到同样效果
  //限定x区域
  x = linspace(-2*pi, 2*pi)
  
  //一张图分成多张图,下一个绘图命令在n行m列第K个格子上绘图
  subplot(n, m, k)
  
  //添加网格线
  Grid on
  
  //更多内容访问https://ww2.mathworks.cn/help/matlab/ref/plot.html
  ~~~

  ![matplot](D:\Xiao\学习笔记\picture\matplot.png)

* fplot：更光滑的plot，语法相同

* polarplot：极坐标

* semilogx：x对数坐标

* semilogy：y对数坐标

* yyaxis：双y轴

* figure：创建图形窗口

* axis：设置范围

## 条形图

* bar: 竖直条形图

* barh：水平条形图

* bar3：三维

* bar3h：三维水平条形图

  ~~~matlab
  bar(y);
  bar(x, y);
  bar(_, width)
  bar(_, style)
  bar(_, color)
  b = bar(_)	//返回bar对象
  ~~~

## 区域图

* area：和plot方法一样
* area(_, basevalue)：basevalue默认0

## 饼图和直方图

* pie：饼图
* pie3
* histogram：直方图
* polarhistogram

## 含误差条的线图

* errorbar(x, y, err)

## 针状图

* stem(x, y)
* stem3(x, y)

## 阶梯图

* stairs(x, y)

## 罗盘图

* compass(u, v)

## 箭头图

* quiver(x, y, u, v)

# 三维绘图

* plot(x, y, z)
* plot(x, y(x), z(x))
* meshgrid(a: b: c): 网格点
* mesh(x, y, z)：网格图

# 例

~~~matlab
clear;clc;close all;
x = linspace(1, 200, 100); %均匀生成1~200
y1 = log(x) + 1;
y2 = log(x) + 2;
figure;
plot(x, y1);
hold on
plot(x, y2, 'LineWidth', 2); %线宽
hold off
legend('y1', 'y2') %图例
~~~

<img src="D:\Xiao\学习笔记\picture\图例1.png" alt="图例1" style="zoom:50%;" />

~~~matlab
figure;
y3 = y1 + rand(1, 100) - 0.5;
plot(x, y1, 'LineWidth', 2, 'Color', [0.21, 0.21, 0.67]);
hold on
plot(x, y3, 'o', 'LineWidth', 2, 'Color', [0.46, 0.63, 0.90], 'MarkerFaceColor', [0.35, 0.90, 0.89], 'MarkerEdgeColor', [0.18, 0.62, 0.17]); %'o'表示散点
hold off
~~~

<img src="D:\Xiao\学习笔记\picture\图例2.png" style="zoom:50%;" />

~~~matlab
x = linspace(0, 3 * pi, 200); %0到3pi分成200点
y = cos(x) + rand(1, 200);	%随机1 * 200个位于[0, 1]的点
sz = 25; %点尺寸25
c = linspace(1, 10, length(x));	%颜色
scatter(x, y, sz, c, 'filled');	%填充圆
~~~

<img src="D:\Xiao\学习笔记\picture\图例4.png" style="zoom:50%;" />

~~~matlab
A = [60.689; 87.714; 143.1; 267.9515];
C = [127.5; 160.4; 231.9; 400.2]
B = C - A;
D = [A, B, C];
bar1 = bar([2: 5: 17], A, 'BarWidth', 0.2, 'FaceColor', 'k');	%轴位置
hold on;
bar2 = bar([3: 5: 18], B, 'BarWidth', 0.2, 'FaceColor', [0.5, 0.5, 0.5]);
hold on;
bar3 = bar([4: 5: 19], C, 'BarWidth', 0.2, 'FaceColor', 'w');
ylabel('耗时/s')
xlabel('GMM阶数')
legend('训练耗时', '测试耗时', '总耗时');
labelID = {'8阶', '16阶', '32阶', '64阶'}
set(gca, 'XTick', 3:5:20);
set(gca, 'XTickLabel', labelID)
~~~

<img src="D:\Xiao\学习笔记\picture\图例5.png" style="zoom:50%;" />

~~~matlab
x = 0.4: 0.1: 2*pi;
y1 = sin(2 * x);
y2 = sin(x);
%确定y1和y2的上下边界
maxY = max([y1; y2]);
minY = min([y1; y2]);
%确定填充多边形，按照顺时针方向来确定点
%fliplr实现左右翻转，形成连续轮廓
xFill = [x, fliplr(x)];
yFill = [maxY, fliplr(minY)];
figure
%填充轮廓
fill(xFill, yFill, [0.21, 0.21, 0.67]);
hold on
%绘制轮廓线
plot(x, y1, 'k', 'LineWidth', 2)
plot(x, y2, 'k', 'LineWidth', 2)
hold off
~~~

<img src="D:\Xiao\学习笔记\picture\图例6.png" style="zoom:50%;" />



~~~matlab
figure;
load('accidents.mat', 'hwydata')
ind = 1: 51;
drivers = hwydata(:,5);
yyaxis left		%左坐标轴
scatter(ind, drivers, 'LineWidth', 2);
title('Highway Data');
xlabel('States');
ylabel('Licensed Drivers(thousands)');
pop = hwydata(:,7);
yyaxis right	%右坐标轴
scatter(ind, pop, 'LineWidth', 2);
ylable('Vehicle Milles Traveled(millions)');
~~~

<img src="D:\Xiao\学习笔记\picture\图例7.png" style="zoom:50%;" />

~~~matlab
[x, y] = meshgrid(0:0.1:1, 0:0.1:1);	%网格点采样
u = x;
v = -y;
startx = 0.1: 0.1: 0.9;
starty = ones(size(startx));
%需要获取所有流线的属性
figure;
%该函数使用箭头来直观的显示矢量场，小箭头来表示以该点为起点的向量(u, v)
quiver(x, y, u, v);
%根据矢量绘制流线图
streamline(x, y, u, v, startx, starty);
~~~

<img src="D:\Xiao\学习笔记\picture\图例8.png" style="zoom:50%;" />

~~~matlab
figure;
t = 0: pi/20: 10*pi;
xt = sin(t);
yt = cos(t);
plot3(xt, yt, t, '-o', 'Color', ...
		'b', 'MarterSize', 10);
~~~

<img src="D:\Xiao\学习笔记\picture\图例9.png" style="zoom:50%;" />

