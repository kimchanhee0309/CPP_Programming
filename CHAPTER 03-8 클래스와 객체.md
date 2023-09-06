# C++ 구조체

<details>
<summary>1. C++ 구조체 선언</summary>
<div markdown="1">       

* 클래스와 동일한 구조와 기능을 가짐
* struct 키워드로 선언하며, 멤버 변수와 멤버 함수를 가지고, 접근 지정도 해야 함
```C++
struct structName {
  //디폴트 접근 지정은 public. public 속성의 멤버 변수나 멤버 함수 선언
private:
  //private 속성의 멤버 변수나 멤버 함수 선언
public:
  //public 속성의 멤버 변수나 멤버 함수 선언
protected:
  //protected 속성의 멤버 변수나 멤버 함수 선언
};
```
</div>
</details>

___

<details>
<summary>2. C++ 구조체의 객체 생성</summary></summary>
<div markdown="1">       

* 클래스 객체 선언 방식과 같이 구조체 타입 뒤에 객체 이름을 지정하면 됨
> > structName stdObj; //structName 타입의 구조체 객체 생성
> 
> > struct structName stdObj; //C++에서 컴파일 오류. struct 키워드 사용 불가
</div>
</details>

___

<details>
<summary>3. 구조체와 클래스의 차이점</summary>
<div markdown="1">       

* 클래스와 기능적으로 동일함
  * 멤버 변수뿐 아니라 생성자와 소멸자를 비롯한 멤버 함수를 가질 수 있음
  * 다른 구조체나 클래스에게 상속 가능하고 다른 구조체나 클래스를 상속받을 수도 있음
  * 멤버들은 접근 지정자로 지정되며, 멤버 활용 방법 또한 클래스와 완전히 동일함
* 클래스와의 차이점
  * 클래스의 디폴트 접근 지정은 private
  * 구조체의 디폴트 접근 지정은 public
```C++
struct Circle {
  Circle();
  Circle(int r);
  double getArea();
private:
  int radius;
};

--------------------------------

class Circle {
  int radius;
public:
  Circle();
  Circle(int r);
  double getArea();
};

//두 코드는 동일함!
```

* 예시 코드(Circle 클래스를 C++ 구조체를 이용하여 재작성하기)
```C++
#include <iostream>
using namespace std;

struct StructCircle { //C++ 구조체 선언
private:
  int radius;
public:
  StructCircle(int r) { radius = r; } //구조체의 생성자
  double getArea();
};

double StructCircle::getArea() {
  return 3.14*radius*radius;
}

int main() {
  StructCircle waffle(3);
  cout << "면적은 " << waffle.getArea();
}
```
</div>
</details>
