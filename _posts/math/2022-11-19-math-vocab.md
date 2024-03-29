---
title: '수학 한글, 영어 단어 모음'
last_modified_at: 2023-05-29T11:20
categories:
  - Math
tags:
  - math vocab
  - proof
  - matrix
  - linear algebra
  - vector
toc: true
toc_sticky: true
use_math: true
# datatable: true
---
# Summary 
매일 수학 용어 한글이랑 영어랑 매치가 안되서 찾아보기 귀찮아서 정리합니다. 



# Number Set (수 집합)




| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
| Real Numbers      |      실수    |      $$\mathbb{R} $$      | 
| Natural Numbers |      자연수     |       $$ \mathbb{N} =\{1,2,3,...\} $$ <br /> Natural numbers 에 0을 포함하냐 안하냐 논쟁있다고 들었는데 전 0 포함하지 않는걸로 배웠습니다.  | 
| Integers        |      정수      |       $$ \mathbb{Z}=\{ -4,-3,..,0,1,2,3,...\} $$   | 
|-------------------------------------|
| Rational Numbers   |     유리수    |      $$\mathbb{Q}= \frac{p}{q} \ \ where \ p,q \in \mathbb{Z} \ and \ q \neq 0 $$      | 
| Complex Numbers      |      복소수    |      $$\mathbb{C}, 𝑎+𝑏𝑖  \ where \ 𝑎,𝑏\in\mathbb{R} $$      | 
| Imaginary Numbers      |      허수    |          | 

<!-- ![image](/assets/images/profile_cafe.JPG) -->


## Properties (법칙, 성질)

| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
|      |      미지수    |        | 
|  commutative property   |   교환법칙    |   $$a+b = b+a \ where \ a,b \in \mathbb(R)$$  | 
|  associative property   |   결합법칙    |   $$a+(b+c) = (a+b)+c$$     | 
|  distributive property   |   분배법칙    |   $$(a+b)\times c= a\times c + b\times c$$     | 
|   identity element <br/>  
    neutral element    |   항등원    |   $$a+$$ <span class="highlight"> $$0$$</span> $$= a$$ (real number addition)<br/> $$a\times$$ <span class="highlight">$$1 $$</span>$$ = a$$ (real number multiplication)    | 
|  inverse element  |   역원    |   $$a$$ <span class="highlight">$$-a$$</span> $$= 0$$  (real number addition)<br/> $$a\times$$ <span class="highlight"> $$\frac{1}{a}$$</span> $$ = 1$$  ( $$ \forall a \in \mathbb{R} \setminus \{0\} $$, real number multiplication) | 


# Linear Algebra (선형 대수)

## Matrices (행렬)


| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
| row        |  행          |      | 
| column     |  열          |        | 
| element     |  원소         |        | 
| square matrix of order n |   n차 정방 행렬  |    $$ m=n$$ for an $$m\times n$$ matrix , $$n \times n$$     | 
| Diagonal matrix      |      대각 행렬    |      $$a_{ij}=0 \ where \ i\neq j$$      | 
| Scalar matrix      |      스칼라 행렬    |      $$a_{ii}=c \ where \ 1 \leq i \leq n$$      | 
| Identity matrix      |     항등 행렬    |      $$a_{ii}=1 \ where \ 1 \leq i \leq n$$      | 
| unit matrix      |      단위 행렬    |      $$a_{ii}=1 \ where \ 1 \leq i \leq n$$      | 
| Inverse matrix      |      역 행렬    |         | 
| Regular matrix   |      정칙 행렬    |   역 행렬을 가질 수 있는 행렬    | 
| nonsingular matrix   |      비특이 행렬    |   역 행렬을 가질 수 있는 행렬    | 
| Invertible matrix    |      가역 행렬    | 역 행렬을 가질 수 있는 행렬      | 
| lower triangular matrix      |      하삼각 행렬    |      $$a_{ij}=0 \ where \ i \lt j$$      | 
| upper triangular matrix      |      상삼각 행렬    |      $$a_{ij}=0 \ where \ i \gt j$$      | 
| row echelon matrix      |      행제형 행렬    |    0행 아래, 선도원소 1, i 번째 선도원소 i+1보다 왼쪽 $$i\geq 1$$  | 
| reduced row echelon matrix |    소거 행제형 행렬    |   행제형 + 선도원소의 열 중에 선도원소 빼고 모든 원소는 0 | 
| reduced row echelon matrix |    전치 행렬   | For an $n\times m$ matrix $$A$$,  $$A^{T} = (a_{ij}^T)$$ and <br/> $$a_{ij}^T = a_{ji} \ (1 \leq i \leq m, 1 \leq j \leq n$$) | 
| cofactpr matrix  |    여인수 행렬   |  | 
| adjoint matrix  |    수반 행렬   | 여인수행렬의 전치($$T$$) | 
| similar matrix  |    닮은 행렬 <br/>상사 행렬   | | 
| orthogonal matrix  |    직교 행렬   | | 
| symmetric matrix  |    대칭 행렬   | $A = A^T$| 

## Vector (벡터), Vector Space (벡터 공간)


| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
| inner product <br/> dot product | 내적    | | 
|  (orthogonal) projection | 정사영    | | 
| orthogonal vector | 정사영벡터 <br/> 그림자벡터   | | 
| field | 체   | | 
|  basis  |   기저    |     | 
|  standard basis  |   표준 기저    |     | 
|  dimension  |   차원    |  $dimV$ 벡터공간의 차원은 V의 기저(basis)를 구성하는 원소의 개수   | 
|  linear combination  |   일차결합    |    | 
|  unit vector | 단위 벡터 | |
|  linearly independent | 일차 독립하다 | 일차 결합으로 표현 못함|
|  linearly dependent | 일차 종속하다 | 일차 결합으로 표현 가능 |
|  linear transformation  |   선형변환    |    | 
|  kernel  |   핵    |    | 
|  image  |   상    |    | 
|  isomorphic  |    동형    | A와 B는 동형. $A \approx B$  | 
|  change of basis  |    기저 변환    |   | 
|  eigenvalue  |   고유값   |  eigen means 'own' in German | 
|  eigenvector  |   고유벡터   |   | 
|  eigenspace  |   고유공간   |   | 
|  characteristic equation  |   특성방정식   | an eq related to eigenvalue <br/> $$|M - \lambda I | = 0$$ | 
|  inner product space  |   내적공간   |   | 
|  orthogonal set  |   직교백터   | 서로 직교인 $O$아님 벡터들의 집합| 
|  orthonormal set  |  단위직교집합   | 길이가 1인 직교벡터들의 집합  | 
|  orthonormal complement  |  직교보공간,<br/>직교여공간   | 길이가 1인 직교벡터들의 집합  | 



## other def's

| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
| leading element <br/> leading entry   |      선도원소    |         | 
| elementary row operation   |      기본행연산    |   $$R_{i,j}$$ (Row Swap), $$R_{i}(c)$$(Scalar multiplication), <br/> $$R_{i,j}(c)$$ (Row Sum, add a multiple of one row to another row)  | 
| row-equivalent    |      행상등    |   matrices $$A$$ and $$B$$ are row-equivalent <br/> if $$B$$ can be achieved from elementary operations on the matrix $$A$$. <br/> $$A$$---> $$A_{1}$$---> $$A_{2}$$ ... ---> $$B$$| 
|  Elimination <br/> method of exhaustion    |      소거법    | | 
| Gaussian Elimination    |     가우스 소거법    | 행제형 행렬 구하고, 후진대입법| 
| Gauss-Jordan Elimination    |     가우스-조르단 소거법    | 소거행제형 행렬 구하고 바로 해 구함| 
| A system of linear equations    | 일차 연립 방정식    | | 
| determinant    | 행렬식    | $$\|A\|$$, square matrix $$A$$에 실수 값 1개를 정한것 $$f: \mathbb{M}_{n\times n} -> \mathbb{R}$$| 
| cofactor expansion    | 여인수 전개    | 행렬식의 여인수 전개| 
| Laplace expansion    | 라플라스 전개    | 행렬식의 라플라스 전개| 
| minor  |    소행렬식  |  $$A$$가 n차 정방행렬일 때, A 의 (i,j) 소행렬식 $$M_{ij}$$는 i행, j  열 없앤 (n-1)차 정방행렬의 행렬식 값| 
|  cofactor     |     여인수    |    the signed minor used to find the inverse of the matrix, adjoined <br/> $$A_{ij} = (-1)^{i+j} det M_{ij}$$   | 
| ordered pair  | 순서조    | | 






# Proof (증명)

| English    | Korean      |  Description   |  
|:-----------|:------------|:---------------|
|  Definition      |      정의    |         | 
|  Axiom      |      공리    |         | 
|  Theorem      |      정리    |         | 
|  Proposition      |      명제    |        | 
|  Lemma      |      부명제    |         | 
|  Corollary      |    따름 정리    |   Theorem에 따름     | 
|  proof by contradiction       |      모순 증명법 <br/> 귀류법    |      | 
|  Direct Proof      |      직접 증명법    |         | 
|  Mathematical Induction      |      수학적 귀납법    |   n=1, n=k    | 
|  sufficient condition <br/> if (=>)    |    충분 조건    |    <span class="highlight">$A$</span>$ \implies B$  일때,<br/> A: B를 만족하기 위한 충분조건<br/>(A만 만족하면, B를 만족하기에 충분)   | 
|  necessary condition  <br/> only if (<=)     |     필요조건    |    $ A \implies$  <span class="highlight">$B$</span>  일때, <br/> B: A를 만족하기 위한 필요조건 <br/>(A를 만족하려면 우선 B의 만족이 필요)   | 
|  if and only if <br/> iff      |     필요충분조건    |    <span class="highlight"> $\iff$</span> <br/>  $ B \implies A \ and \ A \implies B$      | 
|  Therefore      |          |    $\therefore$     | 
|  because      |          |    $\because$     | 
|  there exists      |          |    $\exists$     | 
|  there exists unique     |          |    $!\exists$     | 
<!-- |  equivalent    |     동치     |        |  -->



동치



# Others



| English       | Korean      |  Description   |  
|:--------------|:------------|:---------------|
|               |     미지수    |        | 
|  square       |      자승     |   제곱,같은 수 두 번 곱함     | 
|  polynomial   |     다항식    |       | 
|  coefficient   |     계수    |   $5x + 10$ 에서 $5$    | 
|  term   |     항    |     $ 8x + 9 $ 에서 $8x$ 와 $9$   | 
|  continuous function   |     연속함수    |       | 
|  domain  |     정의역    |   $$f: X->Y$$ 일때 시작점 $$X$$ 집합.   | 
|  codomain <br/>
   target set   |     공역, <br/> 공변역    |  $$f: X->Y$$ 일때 끝점 $$Y$$ 집합. <br/> the set of all possible values which can come out as a result. it refers to the def of a fxn.  | 
|  image  |     상    |    $$y=f(x)$$ 일 때, y는 x의 image(상) <br/> 어떤 함수에 대한 정의역의 원소(들)에 대응하는 공역의 원소(들)   | 
|  inverse image <br/> preimage   |    역상, <br/> 원상   |    어떤 함수에 대한 공역의 원소(들)에 대응하는 정의역의 원소(들). <br/> $$y=f(x)$$ 일 때, x는 y의 inverse image(역상) | 
|  range  |     치역    |    $$X$$ 집합에 속한 모든 원소들의 image를 모아둔 집합. <br/>  the set of values which actually come out. refers to the image of a function  | 
|  mapping       |      사상     |    |
|  surjection       |      전사사상     | $f: A -> B$ 일때,  $f(A) = B$.<br/> range=codomain|
|  injection       |      단사사상     | $f: A -> B$ 일때,  $\forall b \in B, f^{-1}(b) =  \emptyset or 1개$ 원소이면 $f$ 는 단사사상. |
|  bijection       |      전단사사상     |    |
|  coordinate system    |      좌표계     |    |
|  Orthogonal coordinates    |      직교 좌표계     |   |
| least-squares solution   | 최소자승해    | | 
| intercept   | 절편     | 일차 함수  $y=mx+b$ 에서 $b$는 y 절편 또는 y intercept.| 
| polar coordinates   | 극좌표     | | 

<!-- https://talk.jekyllrb.com/t/how-do-i-embed-javascript-in-jekyll/4374/12 -->


