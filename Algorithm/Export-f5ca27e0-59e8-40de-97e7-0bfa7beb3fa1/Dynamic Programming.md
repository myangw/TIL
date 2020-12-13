# Dynamic Programming

(이름만 듣고 뭔가 dynamic하고 하루이상 걸려서 공부해야하는거라 생각했는데, 개념자체는 생각보다 쉬움ㅜ)

freecodecamp유툽영상 듣는중:  [https://youtu.be/oBt53YbR9Kk](https://youtu.be/oBt53YbR9Kk)   이거보다 친절하게 설명하기 어려울 듯 하다. 첫번째 댓글 너무 웃겨..

정리하려고 본 

[이 글]: https://jarednielsen.com/dynamic-programming-memoization-tabulation/(https://jarednielsen.com/dynamic-programming-memoization-tabulation/)

도 친절하다.

## What's DP?

복잡한 문제를 여러개의 하위 문제로 나눠서 풀고, 그것들을 합쳐서 최종 solution을 만들어내는 방법이다. 나중의 쓰임을 위해 값을 캐싱하여 해답을 찾는 과정을 최적화한다.

- Divide and conquer랑 뭐가 달라?
    - dp는 문제를 잘게 쪼갤 때 subproblem이 중복되서 재활용함
    - divide and conquer(분할정복) 은 subproblem은 서로 중복되지 않음(ex. merge sort, quick sort)
- Two approaches:
    - Memoization (top-down)
    - Tabulation(bottom-up)

---

### Memoization

- 이전에 계산해놓은 값을 저장(기억) 해두고 다시 똑같은걸 계산하지 않도록 함 → 전체 실행 속도를 빠르게.
- fibonacci 예시
    - 그냥 짜면 O(2^n)가 된다.  맨 끝 n = 1, n =2인 leaf를 제외하고 모두 두개의 자식노드를 갖는 트리구조가 되므로.

    ```jsx
    const fib = (n) => {
    	if (n <= 2) return 1;
    	return fib(n - 1) + fib(n - 2);
    }
    ```

    - 근데 트리를 그려놓고 보면 중복된 계산들이 있음을 볼 수 있다. fib(7)을 계산할 때 fib(5)를 두번 계산하게 된다. fib(4), fib(3)도 중복 계산해야한다.

        ![Dynamic%20Programming%20d5148e8bc3094001b083b67b1a4f23bf/Untitled.png](Dynamic%20Programming%20d5148e8bc3094001b083b67b1a4f23bf/Untitled.png)

    - ⇒ 중복되는 subproblem의 값들을 저장해두고 재사용할 수 있도록 인자에 object를 추가한다.
        - fib(3) = 2를 계산하고나면 memo= {3: 2}
        - fib(4) = 3을 계산하고나면 memo= {3: 2, 4: 3} ... 이렇게 저장되고, 계산 전에 memo에 값이 있으면 계산하지 않고 memo의 value를 참조한다.

    ```jsx
    const fib = (n, memo = {}) => {
    	if (n in memo) return memo[n];
    	if (n <= 2) return 1;
    	memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
    	return memo[n];
    }
    ```

    - ⇒ time complexity는 O(n)이 됨!
        - fib(7)= fib(6), fib(5) 둘다 계산하던거를 fib(6)만 계산하면 된다. fib(6)은 fib(5)만 계산하면 된다 ...  n이 증가함에 따라 linear하게 증가한다.

### Tabulation

- 가장 작은 subproblem부터 시작해서 그것을 더 큰걸 계산할 때 사용한다.

- Tabulation은 사전적 정의로 '표' 라는 뜻인데, 표에 하나 하나 값을 채워넣고 그걸로 함수를 적용하듯이 그런 기법이라서?? 명명된듯

- 반복문으로 제일 작은 부분부터 계산해가는 방식.

- 피보나치를 tabulation으로도 풀수 있다.

  ```javascript
  const fib = n => {
      if (n === 0) {
          return 0;
      }
      let x = 0;
      let y = 1;
      for (let i = 2; i < n; i++) {
          let tmp = x + y;
          x = y;
          y = tmp;
      }
      return x + y;
  }
  ```

  => 시간복잡도는 O(n), 공간복잡도는 O(1) (재귀호출을 하지 않는다는 장점이 있음.)

