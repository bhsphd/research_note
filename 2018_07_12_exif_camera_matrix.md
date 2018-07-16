### Camera 内参矩阵
$$
Z_c
\left[
\begin{matrix}
u\\
v\\
1
\end{matrix}   
\right]=
\left[
\begin{matrix}
\frac{1}{dx} &\gamma &u_0 \\
0 &\frac{1}{dy}&v_0\\
0 &0 &1
\end{matrix}   
\right]
\left[
\begin{matrix}
f&0&0\\
0&f&0\\
0&0&1
\end{matrix}   
\right][R|t]
\left[
\begin{matrix}
X_w\\
Y_w\\
Z_w\\
1
\end{matrix}   
\right]
$$
$d_x,d_y$单位为$mm/pix$, $f$单位为$mm$,$u_0,v_0$一般接近图像大小的一半.

### 程序中用的内参矩阵$K$
$$
K=
\left[
\begin{matrix}
f_x&s&u_0\\
0 &f_y&v_0\\
0&0&1
\end{matrix}
\right]
$$
其中$f_x,f_y,u_0,v_0$的单位都是像素.


### colmap中相机参数的处理(来自官方文档)



### FocalPlaneXResolution:
Indicate the number of pixels in the image width(X) direction per FocalPlaneResolutionUnit one the camer focal plane.

###FlocalPlaneYResolution:
Indicate the number of pixels in the image height(Y) direction per FocalPlaneResolutionUnit one the camer focal plane.

###ocalPlaneResolutionUnit:
Indicate the unit for measuring FocalPlaneXResolution and FlocalPlaneYResolution.This value is the same as the ResolutionUnit


###focal_length_35mm到实际像素焦距的变换
$35mm$焦距为$focal\_length35(mm)$，实际像素焦距为$focal\_length(pix)$,换算关系如下:

$$
focal\_length=\frac{focal\_length35}{35.0}*max\{image\_width,image\_height\}
$$

###实际物理焦距到像素焦距的变换
$$
focal\_length = \frac{focal\_length(mm)}{sensor\_width(mm)}*max\{image\_width,image\_height\}
$$
