a b c d


Une option américaine est caractérisée par un payoff $U\left(
t\right)=\left( S_{t}-K\right) ^{+}$ pour un call (ou $\left( K-S_{t}\right)
^{+}$ pour un put) payable au détenteur de l'option au temps d'arrêt $\tau
\leq T$ choisi par le détenteur.

On note :

\begin{itemize}
\item $V\left( t\right) $ la valeur de cette option en $t$.

\item $\mathcal{D}\left( t\right) $ l'ensemble des dates d'exercices superieures ou égales à $t$.

\item $\mathcal{T}\left( t\right) $ l'ensemble des temps d'arrêts au temps $
t $ à valeurs dans $\mathcal{D}\left( t\right) $.
\end{itemize}

Soit $N$ un numéraire. Soit $\tau $ un temps d'arrêt à valeur dans $\mathcal{
D}\left( 0\right) $. On note $V^{\tau }\left( 0\right) $ la valeur en $t=0$
de l'option payant $U\left( \tau \right) $ en $\tau $. Nous avons alors :
\begin{equation*}
\frac{V^{\tau }\left( 0\right) }{N\left( 0\right) }=E^{N}\left[ \frac{
U\left( \tau \right) }{N\left( \tau \right) }\right]
\end{equation*}
Par absence d'arbitrage, nous devons avoir :
\begin{equation*}
V\left( 0\right) =\sup_{\tau \in \mathcal{T}\left( 0\right) }V^{\tau }\left(
0\right) =\sup_{\tau \in \mathcal{T}\left( 0\right) }N\left( 0\right) E^{N}
\left[ \frac{U\left( \tau \right) }{N\left( \tau \right) }\right]
\end{equation*}
Ce qui se généralise à :
\begin{equation*}
\frac{V\left( t\right) }{N\left( t\right) }=\sup_{\tau \in \mathcal{T}\left(
t\right) }E^{N}\left[ \frac{U\left( \tau \right) }{N\left( \tau \right) }
\right]
\end{equation*}

