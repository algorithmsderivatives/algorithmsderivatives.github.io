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
\end{equation*}%
D'après Itô : 
\begin{equation*}
\frac{d\xi _{t}^{k}}{\xi _{t}^{k}}=\frac{d\left( 1+\delta
_{k}L_{t}^{k}\right) }{1+\delta _{k}L_{t}^{k}}=\frac{\delta _{k}L_{t}^{k}}{%
1+\delta _{k}L_{t}^{k}}\,\sigma _{t}^{k}\cdot dW_{t}^{k+1}
\end{equation*}%
Notons que $\rho _{t}^{k}$ est une martingale exponentielle sous $Q^{k+1}$ : 
\begin{equation*}
\frac{d\xi _{t}^{k}}{\xi _{t}^{k}}=\alpha _{t}^{k}\cdot dW_{t}^{k+1}
\end{equation*}%
avec%
\begin{equation*}
\alpha _{t}^{k}=\frac{\delta _{k}L_{t}^{k}}{1+\delta _{k}L_{t}^{k}}\,\sigma
_{t}^{k}
\end{equation*}%
D'après le théorème de Girsanov (pour $1\leq l\leq d$) :%
\begin{equation*}
W_{l,t}^{k}=W_{l,t}^{k+1}-\left\langle \int_{0}^{\cdot }\alpha _{u}^{k}\cdot
W_{u}^{k+1},W_{l,u}^{k+1}\right\rangle _{t}=W_{l,t}^{k+1}-\int_{0}^{t}\alpha
_{l,u}^{k}du
\end{equation*}%
$W_{l}^{k}$ est un mouvement brownien sous $Q^{k}$, i.e.%
\begin{equation*}
W_{t}^{k}=W_{t}^{k+1}-\int_{0}^{t}\alpha _{u}^{k}du
\end{equation*}
