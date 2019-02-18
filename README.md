1
2 TEST
3
4


Covariances

Nous avons :
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

En particulier :
\begin{equation*}
\text{var}\left( \ln L_{t}^{i}\right) =\int_{0}^{t}|\sigma _{t}^{i}|^{2}dt
\end{equation*}

donc%
\begin{equation*}
\text{corr}\left( \ln L_{t}^{i},\ln L_{t}^{j}\right) =\frac{\int_{0}^{t}\rho
_{ij}|\sigma _{i}(t)||\sigma _{j}(t)|dt}{\left( \int_{0}^{t}|\sigma
_{i}(t)|^{2}dt\times \int_{0}^{t}|\sigma _{j}(t)|^{2}dt\right) ^{1/2}}
\end{equation*}
