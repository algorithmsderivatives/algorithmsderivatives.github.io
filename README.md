a b c d


Dans le cas d'une option bermuda\textbf{,} on suppose que $\mathcal{D}\left(
0\right) =\left\{ T_{1},T_{2},\ldots ,T_{B}\right\}$ avec $T_{1}>0$ et $
T_{B}=T$.

Pour $t<T_{i+1}$, on définit $H_{i}\left( t\right) $ comme la valeur en $t$
de l'option bermuda quand l'exercice est restreint aux dates $\mathcal{D}
\left( T_{i+1}\right) =\left\{ T_{i+1},T_{i+2},\ldots ,T_{B}\right\} $.\
Comme $H_{i}$ coincide avec $V$ en $t=T_{i+1}$, nous avons :
\begin{equation*}
\frac{H_{i}\left( t\right) }{N\left( t\right) }=E_{t}^{N}\left[ \frac{
V\left( T_{i+1}\right) }{N\left( T_{i+1}\right) }\right] ,\quad i=1,\ldots
,B-1
\end{equation*}
Notons que : $H_{B-1}\left( T_{B}\right) =V\left( T_{B}\right) =U\left(
T_{B}\right) $. A l'instant $T_{i}$, $H_{i}\left( T_{i}\right) $ s'interpre
te comme la valeur de l'option bermuda si l'on n'exerce pas en $T_{i}$.

Si la politique d'exercice optimal est suivie, on doit avoir à l'instant $
T_{i}$ :
\begin{equation*}
V\left( T_{i}\right) =\max \left( U\left( T_{i}\right) ,H_{i}\left(
T_{i}\right) \right) ,\quad i=1,\ldots ,B-1
\end{equation*}
On peut donc écrire :
\begin{equation*}
\frac{H_{i}\left( t\right) }{N\left( t\right) }=E_{t}^{N}\left[ \frac{\max
\left( U\left( T_{i+1}\right) ,H_{i+1}\left( T_{i+1}\right) \right) }{
N\left( T_{i+1}\right) }\right] ,\quad i=0,\ldots ,B-2
\end{equation*}
Le prix de la bermuda s'option par récurrence backward en temps en partant
de la condition terminale $H_{B-1}\left( T_{B}\right) =U\left( T_{B}\right) $
. Et l'on a donc :
\begin{equation*}
V\left( 0\right) =H_{0}\left( 0\right)
\end{equation*}
