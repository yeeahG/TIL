- 에포크 tol  
SGDClassifier는 일정 에포크 동안 성능이 향상되지 않으면 더 훈련하지 않고 자동으로 멈춘다.   
그래서 tol 매개변수에서 향상될 최솟값을 지정하는데 None으로 지정하게 되면 일정 성능까지 향상되어도 max-iter로 지정한 횟수만큼 훈련횟수를 다 마치게 된다

- 정규화(Normalization)와 표준화(Standardization)를 하는 이유
https://lucian-blog.tistory.com/106

- 
