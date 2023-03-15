# Q1
## A1:
$$
\begin{aligned}
\mathbf{v'}=&M\mathbf v\\
=&\begin{bmatrix}
0.866 & -0.500 & 0.000 & 11.0 \\
0.500 & 0.866 & 0.000 & -3.0 \\
0.000 & 0.000 & 1.000 & 9.0 \\
0 & 0 & 0 & 1 
\end{bmatrix} 
\begin{bmatrix}
5.0 \\
10.0 \\
8.0\\
1.0
\end{bmatrix}\\
=&\begin{bmatrix}
10.33\\
8.16\\
17\\
1
\end{bmatrix}
\end{aligned}
$$
故变换后点$\mathbf v=\mathbf v'=[10.33 \quad 8.16 \quad 17 \quad 1]^T$。
# Q2
## A2:
三次变换均为在同一坐标系下变换，故为依次左乘。
$$
\begin{aligned}
&M=Translate^{Y_A}(6)\times Translate^{X_A}(12)\times Rotate^{Z_A}(30\degree)
\\
&Translate^{Y_A}(6)=\begin{bmatrix}
1 &0 &0 &0 \\
0 &1 &0 &6 \\
0 &0 &1 &0 \\
0 &0 &0 &1
\end{bmatrix}
\\

&Translate^{X_A}(12)=\begin{bmatrix}
1 &0 &0 &12 \\
0 &1 &0 &0 \\
0 &0 &1 &0 \\
0 &0 &0 &1
\end{bmatrix}
\\
&Rotate^{Z_A}(30\degree)=\begin{bmatrix}
cos 30\degree &-sin30\degree &0 &0 \\
sin30\degree &cos30\degree &0 &0 \\
0 &0 &1 &0\\
0 &0 &0 &1
\end{bmatrix}

\\
&M=\begin{bmatrix}
0.866 & -0.5 & 0 & 12 \\
0.5   & 0.866& 0 & 6 \\
0     & 0    & 1 & 0 \\
0     & 0    &0  & 1
\end{bmatrix}
\end{aligned}
$$
经计算,$^AP=M\times{^BP}=[\frac{3\sqrt3-1}{2}\quad \frac{7\sqrt3+3}{2}\quad 0\quad 1]^T$