$$
\begin{align*}
f(x)&=\theta^T*x \\
g(x)&= w^T*\varphi(f(x)) \\
&= w^T*\varphi(\theta^T*x) \\
\end{align*}
$$


$$
\frac{\partial g}{\partial w}=\varphi(f(x))
$$

$$
\frac{\partial g}{\partial \varphi}=I_n\bigotimes w
$$

$$
vec({\rm d}\varphi)=vec(\partial \varphi\bigodot{\rm d}f)=diag(\partial \varphi)\bigodot{\rm d}f
$$

$$
\frac{\partial f}{\partial \theta}=x\bigotimes I_m
$$

$$
\begin{align*}
\frac{\partial g}{\partial \theta}&=\frac{\partial g}{\partial \varphi}*\frac{\partial \varphi}{\partial f}*\frac{\partial f}{\partial \theta} \\
&=w*\partial \varphi(f)*x
\end{align*}
$$


$$
loss=\sum_i(g(x^{(i)})-y^{(i)})^2
$$

$$
\frac{\partial loss}{\partial g}=g(x)-y
$$


$$
\begin{align*}
{\rm d}loss&=tr{\left((\frac{\partial loss}{\partial g})^T*{\rm d}g\right)} \\
&=tr{\left((g(x)-y)^T*(\frac{\partial g}{\partial w})^T*{\rm d}w\right)} \\
&=tr{\left((g(x)-y)^T*\varphi^T(x)*{\rm d}w\right)} \\
\end{align*}
$$

$$
\begin{align*}
{\rm d}loss&={\left(\frac{\partial loss}{\partial g}\right)}^T*vec({\rm d}g) \\
&=(g(x)-y)^T*vec({\rm d}g) \\
&=(g(x)-y)^T*I_n\bigotimes w^T*vec({\rm d}\varphi) \\
&=(g(x)-y)^T\bigotimes w^T*vec({\rm d}\varphi) \\
&=(g(x)-y)^T\bigotimes w^T*vec(\partial\varphi\bigodot{\rm d}f) \\
&=(g(x)-y)^T\bigotimes w^T*diag(\partial \varphi)*x\bigotimes I_m* vec({\rm d}\theta) \\
\end{align*}
$$

---

$$
\begin{align*}
\frac{\partial loss}{\partial \theta}&=\sum_i (g(x^{(i)})-y^{(i)})\frac{\partial g^{(i)}}{\partial \theta} \\
&=(g(x)-y)\frac{\partial g}{\partial \theta} \\
&=\sum_i (g^{(i)}-y^{(i)})*w*\partial \varphi(f^{(i)})*x^{(i)} \\
\end{align*}
$$


$$
\begin{align*}
w&:=w+\frac{\partial loss}{\partial w} \\
\end{align*}
$$


$$
\begin{align*}
\frac{\partial loss(w,x)}{\partial w}&=\frac{\partial trM[(h_2(w,x)-y)^2]}{\partial w} \\
&=2*(h_2(x)-y)\frac{\partial h_2}{\partial w} \\
\end{align*}
$$


---
$$
\begin{pmatrix} 1 & -1 \\ \end{pmatrix}* \begin{pmatrix} x \\ 1 \end{pmatrix} = x-1
$$
