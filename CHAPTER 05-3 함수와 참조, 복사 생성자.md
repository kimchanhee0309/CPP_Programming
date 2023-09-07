# 객체 치환 및 객체 리턴

<details>
<summary>1. 객체 치환</summary>
<div markdown="1">       

* 객체 치환 시 모든 데이터가 `비트 단위`로 복사됨
 > Circle c1(5);
 > 
 > 
 > Circle c2(30);
 > 
 > c1 = c2;  //c2 객체를 c1 객체에 비트 단위로 복사한다. c1의 반지름이 30이 된다.
 >
* 객체 치환 후 c1과 c2의 내용이 완전히 같음
  * But, c1과 c2가 하나의 객체가 되는 것이 아니라 별개이며 내용물만 같을 뿐임
  * `객체 치환(assignment)` 은 동일한 클래스 타입에 대해서만 적용됨
</div>
</details>

___

<details>
<summary>2. 함수의 객체 리턴</summary>
<div markdown="1">       

* 함수가 객체를 리턴하는 예시
```C++
Circle getCircle() {
  Circle tmp(30);
  return tmp; //객체 tmp 리턴
}
```
* return문이 실행되면 tmp의 복사본이 생기고, 이 복사본이 getCircle()을 호출한 곳으로 전달됨
* 이후 tmp는 소멸됨
* getCircle() 함수로부터 리턴되는 객체를 받는 코드
  > Circle c; //c의 반지름은 1
  > 
  > 
  > c = getCircle();  //tmp 객체의 복사본이 c에 치환된다. c의 반지름이 30이 된다.
  >
* 객체 c가 생성될 때 반지름 값이 1이었지만,
* getCircle()이 리턴한 tmp 객체로 치환되면 객체 c의 반지름은 30이 됨
* 객체 리턴 예시 코드
```C++
#include <iostream>
using namespace std;

class Circle{
  int radius;
public:
  Circle() { radius = 1; }
  Circle(int radius) { this->radius = radius; }
  void SetRadius(int radius) { this->radius = radius; }
  double getArea() { return 3.14*radius*radius; }
};

Circle getCircle() {
  Circle tmp(30);
  return tmp; //객체 tmp를 리턴한다.
}

int main() {
  Circle c; //객체가 생성된다. radius = 1로 초기화된다.
  cout << c.getArea() << endl;

  c = getCircle();
  cout << c.getArea() << endl;
}

<실행 결과>
3.14
2826
```
</div>
</details>
