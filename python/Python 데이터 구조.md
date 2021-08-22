# 데이터 구조

----

리스트 컴프리헨션의 특징

1. 간결함

2. 성능(일반화의 오류) - 최적화

   *성능비교(loop, comprehension, map)

   - 성능은 파이썬 자체의 변화와 코드 최적화에 따라 얼마든지 달라짐
   - 최신 파이썬부터는 일반 Loop문에도 비약적인 성능 향상이 있었음
   - comprehension은 일반적인 사용에서, map은 built-in 함수를 적용하는 경우에 더 효율적임
   - 성능이란 누가 더 빠르거나 좋다고 일반화할 수 없음

3. 표현력(Pythonic한 표현법)

   



Iterable 타입(list, dict, set, str, bytes, tuple, range) 적용가능한 Built-in Function

>  (0) 반복문 & List Comprehension
>
> (1) map()
>
> (2) filter()
>
> (3) zip()

