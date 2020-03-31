# 유사도(similarities)



## 평균제곱차이 유사도(Mean Squared Difference Similarity)

___________

사용자 *u* 와 사용자 *v* 간의 Mean Squared Difference는 다음과 같이 정의합니다.

**(사용자 *u* 와 사용자 *v* 가 평가한 상품들의 평점간의 차의 제곱)**

**÷ (사용자 *u* 와 사용자 *v* 가 모두 평가한 상품들의 수)**

$msd( u, v ) = {1 \over | I_{uv} |} · \Sigma_{i∈I_{uv}}(r_{ui} - r_{vi})^2$

($I_{uv}$ : 사용자 *u*와 사용자 *v* 모두에 의해 평가된 상품의 집합, $|I_{uv}|$ : 사용자 *u*와 사용자 *v* 모두에 의해 평가된 상품의 수)

또한, 상품 *i*와 상품 *j*간의 Mean Squared Difference는 다음과 같습니다.

$msd( i, j ) = {1 \over | U_{ij}|} · \Sigma_{u∈U_{ij}}(r_{ui} - r_{uj})^2$

($U_{ij}$ : 상품 *i*와 상품 *j* 모두를 평가한 사용자 집합, $|U_{ij}|$ : 상품 *i*와 상품 *j* 모두를 평가한 사용자의 수)



평균 제곱차이 유사도(Mean Squared Difference Similarity)는 Mean Squared Difference(MSD)의 역수로 계싼하는데, 차이가 클 수록 유사도는 작아지고 MSD가 0이 되는 경우를 대응하기 위해 1을 더해줍니다.

$msd\_sim(u, v) = {1 \over {msd(u, v) + 1}}$

$msd\_sim(i,j) = {1 \over {msd(i,j)+1}}$



## 코사인 유사도(Cosine Similarity)

___________

코사인 유사도(Cosine Similarity)는 두 특성 벡터간의 유사 정도를 코사인 값으로 표현한 것입니다. Cosine Similarity는 -1에서 1까지의 값을 가지는데, -1은 벡터의 방향이 서로 완전히 반대되는 경우, 0은 서로 독립적인 경우, 1은 서로 완전히 같은 경우를 의미합니다.

$x·y=|x||y|cos\theta$	→	$cos\theta = {x·y \over {|x|·|y|}}$

이를 Cosine Similarity에 적용하면 사용자 *u*와 사용자 *v*간의 Cosine Similarity는 다음과 같습니다.

$cosine\_sim(u,v) = {{\Sigma_{i∈I_{uv}}r_{ui}·r_{vi}}\over{\sqrt{\Sigma_{i∈I_{uv}}r_{ui}^2}·\sqrt{\Sigma_{i∈I_{uv}}r_{vi}^2}}}$

또한 상품 *i*와 상품 *j*간의 Cosine Similarity는 두 사용자가 모두 평가한 상품의 평점을 사용해서 계산하면 다음과 같습니다.

$cosine\_sim(i,j) = {{\Sigma_{u∈U_{ij}}r_{ui}·r_{uj}}\over{\sqrt{\Sigma_{i∈U_{ij}}r_{ui}^2}·\sqrt{\Sigma_{i∈U_{ij}}r_{uj}^2}}}$



## 피어슨 유사도(Pearson Similarity)

피어슨 유사도는 두 벡터의 상관계수(Pearson correlation coefficient)를 의미하는데 피어슨 유사도는 유사도가 가장 높을 경우 값이 1, 가장 낮을 경우 -1의 값을 가집니다.

사용자 *u*와 사용자 *v*간의 피어슨 유사도는 다음과 같습니다.

$pearson\_sim(u,v) = {\Sigma_{i∈I_{uv}}(r_{ui}-\mu_v)\over{\sqrt{\Sigma_{i∈I_{uv}}(r_{ui}-\mu_u)^2}\sqrt{\Sigma_{i∈I_{uv}}(r_{vi}-\mu_v)^2}}}$

( $\mu_u$ : 사용자 $u$의 평균 평점 )

상품 $i$와 상품 $j$간의 피어슨 유사도는 다음과 같습니다.

$pearson\_sim(i,j) = {\Sigma_{u∈U_{ij}}(r_{ui}-\mu_i)·(r_{uj}-\mu_j)\over{\sqrt{\Sigma_{u∈U_{ij}}(r_{ui}-\mu_i)^2}\sqrt{\Sigma_{u∈U_{ij}}(r_{ui}-\mu_j)^2}}}$

( $\mu_i$ : 상품 $i$의 평균 평점)