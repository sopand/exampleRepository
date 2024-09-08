### 변수 선언
> - let test: string = 'hi';
> - let num: number = 10;
> - let user: object = { name :'capt' , age:100 }
> - 위와 같이 :를 이용하여 자바스크립트 코드에 타입을 정의하는 방식을 타입 표기( Type Annotation ) 라고 합니다.

### 배열 ( Array )
> - let arr : number[] = [1,2,3];
> - let arr : Array<number> =[1,2,3];

### 튜플 ( Tuple )
> 튜플은 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식을 의미합니다.
> - let arr : [String,number]=['hi',10];

### 이넘 ( Enum )
> 이넘은 C, Java와 같은 다른 언어에서 흔하게 쓰이는 타입으로 특정 값(상수)들의 집합을 의미합니다.
> ```typescript
> enum Avengers { 
> Capt = 2 ,
> IronMan,
> Thor
> }
> 
> let hero: Avengers = Avengers.Capt;
> let hero: Avengers = Avengers[0];
> // capt를 2로 변경했을경우의 값 idx를 사용자 편의로 변경이 가능함
> let hero: Avengers = Avengers[2]; // Capt
> let hero: Avengers = Avengers[4]; // Thor
>  ```
> 
>

