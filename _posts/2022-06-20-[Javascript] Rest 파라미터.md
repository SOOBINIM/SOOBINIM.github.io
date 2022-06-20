### Rest 파라미터

Rest 파라미터는 Spread 연산자(…)를 사용하여 파라미터를 정의한 것을 의미 한다.

Rest 파라미터를 사용하면 신수를 함수 내부에서 배열로 전달 받을 수 있다.

순차적으로 파라미터와 Rest 파라미터에 할당된다.

```
Function bar (param1, param2, …rest){
	Console.log(param1) // 1
	Console.log(param2) // 2
	Console.log(…rest) // [3,4,5]
}

Bar(1,2,3,4,5)
```

Rest 파라미터는 반드시 마지막 파라미터이어야 한다.
