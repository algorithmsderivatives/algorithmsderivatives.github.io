
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Notations}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

On pose :%
\begin{equation*}
1+\delta _{k}L_{t}^{k}=\frac{B\left( t,T_{k}\right) }{B\left(
t,T_{k+1}\right) }
\end{equation*}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Hypothèses}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Taux forwards}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

La dynamique de $L_{t}^{i}$ :%
\begin{equation*}
\fbox{$\displaystyle\frac{dL_{t}^{i}}{L_{t}^{i}}=\sigma _{t}^{i}\cdot
dW_{t}^{i+1}$}
\end{equation*}

avec $\sigma _{j}$ volatilité de dimension $d$ et $W^{i+1}$ mouvement
brownien de dimension $d$ sous $Q^{i+1}$, probabilité associée au numéraire $%
B\left( .,T_{i+1}\right) $.

Notons :%
\begin{equation*}
\fbox{$\displaystyle\rho _{ij}\overset{\Delta }{=}\frac{\sigma _{i}\cdot
\sigma _{j}}{\left\Vert \sigma _{i}\right\Vert \left\Vert \sigma
_{j}\right\Vert }$}
\end{equation*}%
En particulier :%
\begin{equation*}
d\left\langle W_{k}^{i+1},W_{l}^{i+1}\right\rangle =\delta _{kl}dt
\end{equation*}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Covariances}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

Nous avons :%
\begin{eqnarray*}
\left\langle \frac{dL_{t}^{i}}{L_{t}^{i}},\frac{dL_{t}^{j}}{L_{t}^{j}}%
\right\rangle  &=&\left\langle \sigma _{t}^{i}\cdot dW^{i+1}(t),[\ldots
]dt+\sigma _{t}^{j}\cdot dW^{i+1}(t)\right\rangle  \\
&=&\sigma _{i}(t)\cdot \sigma _{j}(t)dt \\
&=&\rho _{ij}\left( t\right) \left\Vert \sigma _{i}\left( t\right)
\right\Vert \left\Vert \sigma _{j}\left( t\right) \right\Vert dt
\end{eqnarray*}

et%
\begin{eqnarray*}
\text{cov}\left( \ln L_{t}^{i},\ln L_{t}^{j}\right)  &=&\text{cov}\left(
\int_{0}^{t}\sigma _{u}^{i}\cdot dW_{u}^{i+1},\int_{0}^{t}\sigma
_{u}^{j}\cdot dW_{u}^{i+1}\right)  \\
&=&\int_{0}^{t}\sigma _{u}^{i}\cdot \sigma _{u}^{j}du \\
&=&\int_{0}^{t}\rho ^{ij}|\sigma _{t}^{i}||\sigma _{t}^{i}|dt
\end{eqnarray*}

En particulier :%
\begin{equation*}
\text{var}\left( \ln L_{t}^{i}\right) =\int_{0}^{t}|\sigma _{t}^{i}|^{2}dt
\end{equation*}

donc%
\begin{equation*}
\text{corr}\left( \ln L_{t}^{i},\ln L_{t}^{j}\right) =\frac{\int_{0}^{t}\rho
_{ij}|\sigma _{i}(t)||\sigma _{j}(t)|dt}{\left( \int_{0}^{t}|\sigma
_{i}(t)|^{2}dt\times \int_{0}^{t}|\sigma _{j}(t)|^{2}dt\right) ^{1/2}}
\end{equation*}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Changement de mesure}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

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

\begin{prop}
Nous avons :%
\begin{equation*}
\fbox{$\displaystyle dW_{t}^{k}=dW_{t}^{k+1}-\frac{\delta _{k}L_{t}^{k}}{%
1+\delta _{k}L_{t}^{k}}\,\sigma _{t}^{k}dt$}
\end{equation*}%
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
\end{equation*}%
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
En effet%
\begin{eqnarray*}
\frac{dL_{t}^{i}}{L_{t}^{i}} &=&\sigma _{t}^{i}\cdot dW_{t}^{i+1} \\
&=&\sum_{l=1}^{d}\sigma _{i,l}(t)\cdot dw_{l}^{i+1}(t) \\
&=&\sum_{l=1}^{d}\sigma _{i,l}(t)\cdot \left[ dw_{l}^{i}(t)+\frac{\delta
_{i}L_{i}(t)}{1+\delta _{i}L_{i}(t)}\,\sigma _{i,l}(t)dt\right]  \\
&=&\sigma _{i}(t)\cdot dw^{i}(t)+\frac{\delta _{i}L_{i}(t)}{1+\delta
_{i}L_{i}(t)}\,\sigma _{i}(t)\cdot \sigma _{i}(t)dt
\end{eqnarray*}
\end{proof}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsubsection{Mesure terminale}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\begin{prop}
Nous avons :%
\begin{equation*}
\fbox{$\displaystyle\frac{dL_{t}^{i}}{L_{t}^{i}}=\sigma ^{i}\cdot
dW_{t}^{N}-\sum_{k=i+1}^{N-1}\frac{\delta _{k}L_{t}^{k}}{1+\delta
_{k}L_{t}^{k}}\,\sigma _{t}^{i}\cdot \sigma _{t}^{k}dt$}
\end{equation*}%
i.e.%
\begin{equation*}
\frac{dL_{t}^{i}}{L_{t}^{i}}=\sigma _{i}\left( t\right) \cdot dw^{N}\left(
t\right) -\sum_{k=i+1}^{N-1}\frac{\delta _{k}L_{k}\left( t\right) }{1+\delta
_{k}L_{k}\left( t\right) }\,\rho _{ik}\left( t\right) \left\Vert \sigma
_{i}\left( t\right) \right\Vert \left\Vert \sigma _{j}\left( t\right)
\right\Vert dt
\end{equation*}
\end{prop}

\begin{proof}
Notons que : 
\begin{equation*}
dw^{i+1}\left( t\right) =dw^{N}\left( t\right) +\sum_{k=i+1}^{N-1}\left(
dw^{k}\left( t\right) -dw^{k+1}\left( t\right) \right)
\end{equation*}%
Donc : 
\begin{eqnarray*}
\frac{dL_{i}(t)}{L_{i}(t)} &=&\sigma _{i}(t)\cdot dw^{i+1}(t) \\
&=&\sigma _{i}(t)\cdot \left(
dw^{N}(t)+\sum_{k=i+1}^{N-1}(dw^{k}(t)-dw^{k+1}(t))\right) \\
&=&\sigma _{i}(t)\cdot dw^{N}(t)-\sum_{k=i+1}^{N-1}\frac{\delta
_{k}L_{k}\left( t\right) }{1+\delta _{k}L_{k}\left( t\right) }\,\sigma
_{i}\left( t\right) \cdot \sigma _{k}\left( t\right) dt
\end{eqnarray*}
\end{proof}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Taux de swap forwards}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

$S_{\alpha ,\beta }(t)$ est le taux de swap à la date $t$, i.e. le taux fixe
qui annule la valeur du swap à la date $t$ : 
\begin{equation*}
\text{fixedLeg}(t,S_{\alpha ,\beta }(t))=\text{floatLeg}(t)
\end{equation*}%
i.e. 
\begin{equation*}
S_{\alpha ,\beta }(t)=\frac{P(t,T_{\alpha })-P(t,T_{\beta })}{\sum_{i=\alpha
+1}^{\beta }\tau _{i}P(t,T_{i})}
\end{equation*}

\begin{Rem}
Nous avons%
\begin{equation*}
S_{\alpha ,\beta }(t)=\frac{\sum_{i=\alpha +1}^{\beta }\tau
_{i}P(t,T_{i})L_{i}(t)}{\sum_{i=\alpha +1}^{\beta }\tau _{i}P(t,T_{i})}%
=\sum_{i=\alpha +1}^{\beta }\omega _{i}(t)L_{i}(t)
\end{equation*}%
avec 
\begin{equation*}
\omega _{i}(t)=\frac{\tau _{i}P(t,T_{i})}{\sum_{j=\alpha +1}^{\beta }\tau
_{j}P(t,T_{j})}
\end{equation*}
\end{Rem}

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

\begin{equation*}
S_{0,n+1}(t)=\frac{\sum_{i=1}^{n+1}\tau _{i}B(t,T_{i})L(t,T_{i-1},T_{i})}{%
\sum_{i=\alpha +1}^{\beta }\tau _{i}B(t,T_{i})}=\sum_{i=1}^{n+1}\omega
_{i}(t)L(t,T_{i-1},T_{i})
\end{equation*}
