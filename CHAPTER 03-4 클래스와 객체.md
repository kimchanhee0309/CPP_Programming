# CHAPTER 03-4. 생성자
<details>
<summary>1. 생성자란?</summary>
<div markdown="1">       

 * 객체가 생성될 때 자동으로 실행되는 `생성자(constructor)` 라는 특별한 멤버 함수를 통해 **객체를 초기화**함
   * 한 클래스에 여러 개의 생성자를 둘 수 있으나, 이 중 하나만 실행됨
```C++
class Circle
{
  ....
  Circle(); //클래스 이름과 동일
  Circle(int r); //리턴 타입 명기하지 않음
  .......... //2개의 생성자 함수 선언
};

Circle::Circle(){ //매개 변수 없는 생성자 함수 구현
  ........
}

Circle::Circle(int r){ //매개 변수를 가진 생성자 함수 구현
  ........
}
```
 * 생성자의 목적은 객체가 생성될 때 필요한 **초기 작업을 위함**임
 * 생성자 함수는 **오직 한 번만 실행**됨
 * 생성자 함수의 이름은 **클래스 이름과 동일하게 작성**되어야 함
 * 생성자 함수의 원형에 **리턴 타입을 선언하지 않음**
   * void 리턴 타입을 설정해서도 안 됨
   * return문을 사용할 수는 있지만, 어떤 값도 리턴하면 안 됨
 * 예시 코드
```C++
class Circle
{
  ....
  Circle(); //정상적인 생성자 선언. 리턴 타입 선언하지 않음
  void Circle(int r); //컴파일 오류. 생성자는 리턴 타입 없음
  int Circle(double r); //컴파일 오류. 생성자는 리턴 타입 없음
};

Circle::Circle()
{
  ...
  return; //생성자 함수를 종료하는 정상적인 리턴문
}
Circle::Circle()
{
  ...
  return 0; //컴파일 오류. 생성자 함수는 값을 리턴해서는 안 됨
}
```
 * 생성자는 **중복 가능**함
   * 생성자는 한 클래스에 여러 개 만들 수 있지만, **매개 변수 개수나 타입이 서로 다르게 선언**되어야 함
     > Circle(); //매개 변수 없는 생성자
     
     > Circle(int r); //매개 변수 있는 생성자

</div>
</details>

___
<details>
<summary>2. 객체 생성과 생성자 실행</summary>
<div markdown="1">       

</div>
</details>

___
<details>
<summary>3. 위임 생성자(delegating constructor), 생성자가 다른 생성자 호출</summary>
<div markdown="1">       

</div>
</details>

___
<details>
<summary>4. 생성자와 멤버 변수의 초기화</summary>
<div markdown="1">       

</div>
</details>

___
<details>
<summary>5. 생성자는 꼭 있어야 하는가?</summary>
<div markdown="1">       

</div>
</details>

___
<details>
<summary>6. 기본 생성자(매개 변수 없는 생성자)</summary>
<div markdown="1">       

</div>
</details>
