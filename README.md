a b c d

2 TEST
5

Changement de mesure

Posons : 
\begin{equation*}
\xi _{t}^{k}\overset{\Delta }{=}\frac{dQ^{k}}{dQ^{k+1}}|\mathcal{F}%
_{t}=\left. \frac{B\left( t,T_{k}\right) }{B\left( 0,T_{k}\right) }\right/ 
\frac{B\left( t,T_{k+1}\right) }{B\left( 0,T_{k+1}\right) }=\frac{1+\delta
_{k}L_{t}^{k}}{1+\delta _{k}L_{0}^{k}}
\end{equation*}
D'après Itô : 
\begin{equation*}
\frac{d\xi _{t}^{k}}{\xi _{t}^{k}}=\frac{d\left( 1+\delta
_{k}L_{t}^{k}\right) }{1+\delta _{k}L_{t}^{k}}=\frac{\delta _{k}L_{t}^{k}}{%
1+\delta _{k}L_{t}^{k}}\,\sigma _{t}^{k}\cdot dW_{t}^{k+1}
\end{equation*}
Notons que $\rho _{t}^{k}$ est une martingale exponentielle sous $Q^{k+1}$ : 
\begin{equation*}
\frac{d\xi _{t}^{k}}{\xi _{t}^{k}}=\alpha _{t}^{k}\cdot dW_{t}^{k+1}
\end{equation*}
avec%
\begin{equation*}
\alpha _{t}^{k}=\frac{\delta _{k}L_{t}^{k}}{1+\delta _{k}L_{t}^{k}}\,\sigma
_{t}^{k}
\end{equation*}
D'après le théorème de Girsanov (pour $1\leq l\leq d$) :%
\begin{equation*}
W_{l,t}^{k}=W_{l,t}^{k+1}-\left\langle \int_{0}^{\cdot }\alpha _{u}^{k}\cdot
W_{u}^{k+1},W_{l,u}^{k+1}\right\rangle _{t}=W_{l,t}^{k+1}-\int_{0}^{t}\alpha
_{l,u}^{k}du
\end{equation*}
$W_{l}^{k}$ est un mouvement brownien sous $Q^{k}$, i.e.%
\begin{equation*}
W_{t}^{k}=W_{t}^{k+1}-\int_{0}^{t}\alpha _{u}^{k}du
\end{equation*}

\begin{prop}
Nous avons :
\begin{equation*}
\fbox{$\displaystyle dW_{t}^{k}=dW_{t}^{k+1}-\frac{\delta _{k}L_{t}^{k}}{%
1+\delta _{k}L_{t}^{k}}\,\sigma _{t}^{k}dt$}
\end{equation*}
Notons que : 
\begin{equation*}
dW_{t}^{i+1}=dW^{N}\left( t\right) +\sum_{k=i+1}^{N-1}\left( dW^{k}\left(
t\right) -dW^{k+1}\left( t\right) \right) =dw^{N}\left( t\right)
+\sum_{k=i+1}^{N-1}\frac{\delta _{k}L_{k}\left( t\right) }{1+\delta
_{k}L_{k}\left( t\right) }\,\sigma _{k}\left( t\right) dt
\end{equation*}
\end{prop}

\begin{proof}
Et 
\begin{equation*}
dW_{l}^{k}\left( t\right) =dW_{l}^{k+1}\left( t\right) -d\left\langle \frac{%
\delta _{k}L_{k}\left( t\right) }{1+\delta _{k}L_{k}\left( t\right) }%
\,\sigma _{k}(t)\cdot w^{k+1},w_{l}^{k+1}\right\rangle
_{t}=dw_{l}^{k+1}\left( t\right) -\frac{\delta _{k}L_{k}\left( t\right) }{%
1+\delta _{k}L_{k}\left( t\right) }\,\sigma _{k,l}\left( t\right) dt
\end{equation*}
En supposant que : $d\left\langle W_{j}^{k+1},W_{l}^{k+1}\right\rangle
=\delta _{jl}dt$ i.e. 
\begin{equation*}
dw^{k}\left( t\right) =dw^{k+1}\left( t\right) -\frac{\delta _{k}L_{k}(t)}{%
1+\delta _{k}L_{k}(t)}\,\sigma _{k}\left( t\right) dt
\end{equation*}
\end{proof}

\begin{prop}
En particulier, le libor $L_{t}^{i}$ s'écrit sous $Q^{T_{i}}$ : 
\begin{equation*}
\frac{dL_{t}^{i}}{L_{t}^{i}}=\sigma _{t}^{i}\cdot dW_{t}^{i}+\frac{\delta
_{i}L_{t}^{i}}{1+\delta _{i}L_{t}^{i}}\,\sigma _{t}^{i}\cdot \sigma
_{t}^{i}dt
\end{equation*}
\end{prop}

\begin{proof}
En effet
\begin{eqnarray*}
\frac{dL_{t}^{i}}{L_{t}^{i}} &=&\sigma _{t}^{i}\cdot dW_{t}^{i+1} \\
&=&\sum_{l=1}^{d}\sigma _{i,l}(t)\cdot dw_{l}^{i+1}(t) \\
&=&\sum_{l=1}^{d}\sigma _{i,l}(t)\cdot \left[ dw_{l}^{i}(t)+\frac{\delta
_{i}L_{i}(t)}{1+\delta _{i}L_{i}(t)}\,\sigma _{i,l}(t)dt\right]  \\
&=&\sigma _{i}(t)\cdot dw^{i}(t)+\frac{\delta _{i}L_{i}(t)}{1+\delta
_{i}L_{i}(t)}\,\sigma _{i}(t)\cdot \sigma _{i}(t)dt
\end{eqnarray*}
\end{proof}
