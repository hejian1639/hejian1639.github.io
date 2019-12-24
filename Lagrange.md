
$$ 
\begin{align*}
& \min f(x) \\
s.t. & \\
& h(x)=0 \\
& g(x)\leq0 
\end{align*}
$$


$$
L(x,\lambda)=f(x)+\lambda_0 h(x)+\lambda^T g(x)
$$

$$
L'(x,\lambda)=0 \\
\lambda_i*g_i(x)=0
$$



---

0 1背包问题

$$
\begin{align*}
& \max v^T*x \\
s.t. & \\
& w^T*x\leq w_0 \\
& x\leq E
\end{align*}
$$

$$
L(x,\lambda)=-v^T*x+\lambda_0*(w^T*x-w_0)+\lambda^T*(x-E)
$$

$$
\begin{align*}
&v_i=\lambda_0*w_i+\lambda_i \\
&\lambda_0*(w^T*x-w_0)=0 \\
&\lambda_i*(x_i-1)=0
\end{align*}
$$

$$
\begin{align*}
L(x,\lambda)=& -v^T*x+\lambda_0*(w^T*x-w_0)+\lambda^T*(x-E) \\
=& -(\lambda_0*w^T+\lambda^T)*x+\lambda_0*(w^T*x-w_0)+\lambda^T*(x-E) \\
=& -\lambda_0*w_0-\lambda^T*E
\end{align*}
$$

---

$$
\begin{align*}
&\min \lambda_0*w_0+\sum\lambda_i \\
s.t. & \\
&v_i=\lambda_0*w_i+\lambda_i \\
\end{align*}
$$

---




