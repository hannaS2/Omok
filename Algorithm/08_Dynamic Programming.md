# Dynamic Programming :동적 계획법
최적화 문제를 연구하는 수학 이론에서 왔으며, 전산학 전반에서 일반적으로 사용하는 동적, 프로그래밍이라는 단어와 아무런 관련 없는 동적계획법이다.  
큰 의미에서는 분할 정복과 같은 접근 방식을 의미한다. 처음 주어진 문제를 더 작은 문제들로 나눈 뒤 각 조각의 답을 계산하고, 이 답들로부터 원래 문제에 대한 답을 계산하는 것은 같지만, 차이점은 문제를 나누는 방식이다.  
동적 계획법에서 어떤 부분 문제는 두 개 이상의 문제를 푸는데 중복되어 사용될 수 있기 때문에 단순한 재귀 호출로 여러 번 계산하는 대신 각 문제의 답을 메모리에 저장해 한 번만 계산하고 계산 결과를 재활용함으로써 속도를 향상시킬 수 있다. 이때, 이미 계산한 값을 저장해 두는 메모리의 장소를 캐시(cache)라고 부르고, 두 번 이상 계산되는 부분 문제를 중복되는 부분 문제(overlapping subproblems)라고 부른다.

- 메모이제이션 (memoization)  
함수의 결과를 저장하는 장소를 마련해 두고, 한 번 계산한 값을 저장해 뒀다 재활용하는 최적화 기법  
메모이제이션을 사용하면 모든 부분 문제가 한 번씩만 계산되므로 함수 호출 횟수가 엄청나게 감소한다.  
캐싱이 필요하기 때문에 메모이제이션은 입력이 같을 때 출력도 항상 같은 참조적 투명 함수(referential transparent function)의 경우에만 적용할 수 있다.  
시간 복잡도의 상한은 (존재하는 부분 문제의 수)*(한 부분 문제를 풀 때 필요한 반복문의 수행 횟수)로 간단하게 계산할 수 있다.

## 예제
### binomial coefficient (이항 계수)
메모이제이션을 사용하여 한 번 계산한 값을 배열에 저장하고, 함수 호출 시 배열에 값이 저장되어 있는지 확인한 후 저장되어 있다면 재사용하고, 저장되어 있지 않다면 계산한 후 배열에 저장한다.
```java
<top-down>
private static int[][] cache = new int[30][30];

private static int bino(int n, int r) {
    // base-case
    if (r == 0 || n == r) return 1;
    // 0이 아니라면 한 번 계산했던 값이므로 반환
    if (cache[n][r] != 0) return cache[n][r];
    // 없는 경우에는 직접 계산한 뒤 배열에 저장
    cache[n][r] = bino(n - 1, r - 1) + bino(n - 1, r);
    return cache[n][r];
}
```
> r의 최대치는 n이므로 존재할 수 있는 부분 문제의 수는 최대 O(N<sup>2</sup>)  
> 각 부분 문제를 게산할 때 걸리는 시간은 반복문이 없으므로 O(1)  
> 따라서, O(N<sup>2</sup>) * O(1) = **O(N<sup>2</sup>)**

```java
<bottom-up>
private static int[][] cache = new int[30][30];

private static int bino2(int n, int r) {
    // cache[0][0]부터 시작하여 위에서 아래로 재귀 관계식을 적용하여 배열을 채워나간다.
    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= Math.min(i, r); j++) {
            if (j == 0 || j == i) cache[i][j] = 1;
            else cache[i][j] = cache[i - 1][j - 1] + cache[i - 1][j];
        }
    }
    return cache[n][r];
}
```

### 

---
#algorithm/알고리즘문제해결전략  