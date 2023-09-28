# C++ 프렌드 개념

<details>
<summary>1. 친구와 C++ 프렌드</summary>
<div markdown="1">       

* C++에서는 클래스 외부에 작성된 함수를 클래스 내에 friend 키워드로 선언하여, 클래스의 멤버 함수와 동일한 접근 자격을 부여할 수 있음
* 멤버가 아니므로 상속되지는 않음
* 클래스 내에 **friend 키워드**로 선언된 외부 함수를 **프렌드 함수(friend function)** 라고 부르며,
* 프렌드 함수는 클래스의 멤버인 것처럼 클래스의 모든 변수나 함수에 접근할 수 있음
* 프렌드 함수가 필요한 이유?
  * 클래스 멤버 함수로는 적합하지 않지만, 클래스의 private, protected 멤버를 접근해야 하는 경우, 이 함수를 외부 함수로 작성해야 할 때 필요함
* 프렌드 함수를 선언할 수 있는 3가지 경우
  * 클래스 외부에 작성된 함수를 프렌드로 선언
  * 다른 클래스의 멤버 함수를 프렌드로 선언
  * 다른 클래스의 모든 멤버 함수를 프렌드로 선언
</div>
</details>

___

<details>
<summary>2. 프렌드 함수 선언</summary>
<div markdown="1">       

* 클래스 외부에 구현된 함수를 friend 키워드로 클래스 내의 아무 곳에나 선언 가능함
* 예시 코드
```C++
#include <iostream>
using namespace std;

class Rect; 
bool equals(Rect r, Rect s);  //equals() 함수 선언

class Rect { //Rect 클래스 선언
  int width, height;
public:
  Rect(int width, int height) { this->width = width; this->height = height; }
  friend bool equals(Rect r, Rect s);  //프렌드 함수 선언
};

bool equals(Rect r, Rect s) { //외부 함수
  if(r.width == s.width && r.height == s.height) return true;
  else return false;
}

int main() {
  Rect a(3,4), b(4,5);
  if(equals(a, b)) cout << "equal" << endl;
  else cout << "not equal" << endl;

<실행 결과>
not equal
```
</div>
</details>

___

<details>
<summary>3. 프렌드 멤버 선언</summary>
<div markdown="1">       

* **다른 클래스의 멤버 함수**를 클래스의 프렌드 함수로 선언할 수 있음
* 예시 코드
```C++
#include <iostream>
using namespace std;

class Rect;

class RectManager { //RectManager 클래스 선언
public:
  bool equals(Rect r, Rect s);
};

class Rect { //Rect 클래스 선언
  int width, height;
public:
  Rect(int width, int height) { this->width = width; this->height = height; }
  friend bool RectManager::equals(Rect r, Rect s);  //프렌드 함수 선언
};

bool RectManager::equals(Rect r, Rect s) { //RectManager::equals() 구현
  if(r.width == s.width && r.height == s.height) return true;
  else return false;
}

int main() {
  Rect a(3,4), b(3,4);
  RectManager man;

  if(man.equals(a, b)) cout << "equal" << endl;
  else cout << "not equal" << endl;

<실행 결과>
equal
```
</div>
</details>

___

<details>
<summary>4. 프렌드 클래스 선언</summary>
<div markdown="1">       

* **다른 클래스의 모든 멤버 함수**를 클래스의 프렌드 함수로 한 번에 선언할 수 있음
* 예시 코드
```C++
#include <iostream>
using namespace std;

class Rect;

class RectManager { //RectManager 클래스 선언
public:
  bool equals(Rect r, Rect s);
  void copy(Rect& dest, Rect& src);
};

class Rect { //Rect 클래스 선언
  int width, height;
public:
  Rect(int width, int height) { this->width = width; this->height = height; }
  friend RectManager; //RectManager 클래스의 모든 함수를 프렌드 함수로 선언
};

bool Rectmanager::equals(Rect r, Rect s) { //r과 s가 같으면 true 리턴
  if(r.width == s.width && r.height == s.height) return true;
  else return false;
}

void RectManager::copy(Rect& dest, Rect& src) { //src를 dest에 복사
  dest.width = src.width; dest.height = src.height;
}

int main() { 
  Rect a(3,4), b(5,6);
  RectManager man;

  man.copy(b, a);  //a를 b에 복사한다.
  if(man.equlas(a, b)) cout << "equal" << endl;
  else cout << "not equal" << endl;

<실행 결과>
equal
```
</div>
</details>
