# 접근 지정

<details>
<summary>1. 접근 지정자</summary>
<div markdown="1">       

* **private 멤버**
  * 클래스 내의 멤버 함수들에게만 접근이 허용됨
* **public 멤버**
  * 클래스 내외를 막론하고 프로그램의 모든 함수들에게 접근이 허용됨
* **protected 멤버**
  * 클래스 내의 멤버 함수와 이 클래스를 상속받은 파생 클래스의 멤버 함수에게만 접근이 허용됨
</div>
</details>

___

<details>
<summary>2. 디폴트 접근 지정은 private</summary>
<div markdown="1">       

* 접근 지정을 하지 않은 경우, 디폴트 접근 지정은 private으로 처리됨
</div>
</details>

___

<details>
<summary>3. 멤버 보호와 생성자</summary>
<div markdown="1">       

#### 멤버 변수는 private으로 지정하는 것이 바람직
* 클래스의 멤버들은 클래스 외부에서 마음대로 접근할 수 있도록 허용해서는 안 되는 것이 기본임
* 예시 코드
```C++
class Circle {
public:
  int radius; //멤버 변수 보호받지 못함
  Circle();
  Circle(int r);
  double getArea();
};

Circle::Circle() {
  radius = 1;
}
Circle::Circle(int r) {
  radius = r;
}

int main() {
  Circle waffle;
  waffle.radius = 5; //노출된 멤버는 마음대로 접근(안 좋은 사례)
}
```
```C++
class Circle {
private:
  int radius; //멤버 변수 보호받고 있음
  Circle();
  Circle(int r);
  double getArea();
};

Circle::Circle() {
  radius = 1;
}
Circle::Circle(int r) {
  radius = r;
}

int main() {
  Circle waffle(5); //생성자에서 radius 설정
  waffle.radius = 5; //private 멤버 접근 불가
}
```

#### 생성자 public으로
* 클래스 외부에서 객체를 생성하기 위해서는 생성자를 public으로 선언해야 함
</div>
</details>
