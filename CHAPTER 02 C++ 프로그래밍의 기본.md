# CHAPTER 02 C++ 프로그래밍의 기본
### 2.1 C++ 프로그램의 기본 요소와 화면 출력
* **main() 함수**
   * C++ 표준에서 정한 main() 함수의 리턴 타입은 int
   * int main() 함수에서 return문은 생략이 가능함

* **#include <iostream>**
    * 전처리기에 대한 지시문으로서, C++ 소스 파일을 컴파일하기 전에 <iostream> 헤더 파일을 읽어 C++ 소스 파일 안에 삽입할 것을 지시함
    * <iostream> 헤더 파일에는 C++ 표준 입출력을 위한 클래스와 객체가 선언되어 있으므로, 키보드 입력이나 화면 출력을 위해서 꼭 필요함
      
* **화면 출력**
   * cout 객체
     * C++ 표준 출력 스트림 객체로, C++ 프로그램에서 출력한 데이터를 자신과 연결된 스크린에 대신 출력해줌
     * cout 옆에 붙여진 std:: 접두어는 cout의 이름 공간(namespace)이 std임을 표시함
       
   * << 연산자
     * 스트림 삽입 연산자로 불리며, 오른쪽 피연산자 데이터를 왼쪽 스트림 객체에 삽입함
     * 여러 개의 << 연산자로 한 문장에 여러 데이터를 출력할 수 있음
     * 문자열뿐 아니라 C++ 기본 타입 데이터도 출력할 수 있음
```C++
int n=3;
char c='#';
std::cout << c << 5.5 << '-' << n << "hello" << true;

<실행결과>
#5.5-3hello1
``` 

___
### 2.2 namespace와 std::
* **namespace**
   * 이름 충돌이 발생하는 경우
     * 프로젝트를 여러 명이 나누어 개발하는 경우
     * 다른 사람이 작성한 소스 코드나 목적 파일을 가져와서 사용하는 경우
     * 프로젝트를 여러 명이 나누어 개발하는 경우, 한 개발자가 작성한 **클래스, 상수, 변수, 함수의 이름**이 같은 팀의 다른 개발자가 작성한 이름과 동일하면, 함께 컴파일하거나 링크할 때 오류가 발생
     * 이처럼 여러 사람들이 작성한 프로그램에서 변수, 함수, 클래스 등의 이름(identifier)이 충돌하는 것을 막기 위해, 개발자가 자신만의 고유한 이름 공간을 생성할 수 있도록 namespace 키워드를 도입함
   * 이름 공간을 생성하는 방법
```C++
namespace Kitae{
  . . . . .
}
/*생성한 이름을 이용하기 위해서
  이름공간(namespace)::이름(identifier)*/
```
    
* **std::란?**
   * 표준 이름 공간을 뜻함
      
* **std::의 생략과 using 지시어**
   * using namespace std; 선언을 통해 생략 가능함
     
* **#include <iostream>과 std**
```C++
#include <iostream> //cout과 << 연산자 정의 포함
using namespace std; //<iostream> 헤더 파일에 선언된 이름을 사용할 때 std:: 생략

//C++ 프로그램은 main() 함수에서부터 실행을 시작함
int main()
{
  cout << "Hello\n"; //화면에 Hello를 출력하고 다음 줄로 넘어감
  cout << "첫 번째 맛보기입니다."; 
  return 0; //main() 함수가 종료하면 프로그램이 종료됨
}
```

___
### 2.3 키 입력 받기
* **예제 소스**
```C++
#include <iostream>
using namespace std;

int main(){
  cout << "너비를 입력하세요>>";

  int width;
  cin >> width; //키보드로부터 정수 값 너비를 읽어 width 변수에 저장

  cout << "높이를 입력하세요>>";

  int height;
  cin >> height; //키보드로부터 정수 값 높이를 읽어 height 변수에 저장

  int area = width*height; //사각형의 면적 계산
  cout << "면적은 " << area << "\n"; //면적을 출력하고 다음 줄로 넘어감
}
```

* **cin과 >> 연산자를 이용한 키 입력**
   * C++에서는 표준 입력 스트림인 cin과 >> 연산자를 이용하여 사용자로부터 키를 입력받음
     * 스트림 추출 연산자로 불리며, 왼쪽 피연산자인 스트림 객체로부터 데이터를 읽어 오른쪽 피연산자에 지정된 변수에 삽입함
     * 여러 개의 >> 연산자를 이용하여 여러 값을 입력받을 수도 있음 
   * C언어와 달리 키 입력받는 변수 이름 그대로 사용함(주소값 x)
```C++
int width;
cin >> width; //(O)
cin >> &width; //(X), width의 주소 값을 주어서는 안 됨!
```

* **<Enter> 키를 칠 때 변수에 키 값 전달**
   * <Enter> 키를 통해 사용자의 키 입력이 끝났다는 사실을 인식함
      
* **실행문 중간에 변수 선언**
   * 프로그램 어디서나 변수 선언이 가능함
   * 장점
     * 변수를 사용하는 코드 바로 위에 선언할 수 있어 코드를 읽기 쉽게 만들어줌
     * 변수를 사용하기 바로 전 라인에 변수를 선언하면, 변수 이름을 잘못 타이핑하는 실수를 줄일 수 있음 
   * 단점
     * 선언된 모든 변수를 한 눈에 보기 힘들고, 코드 사이에서 선언된 변수를 찾기가 용이하지는 않음
   * 예시 코드
```C++
int width;
cin >> width; //키보드로부터 너비를 읽는다.

cout << "높이를 입력하세요>>";

int height;
cin >> height;; //키보드로부터 높이를 읽는다.

//너비와 높이로 구성되는 사각형의 면적을 계산한다.
int area = width*height;
cout << "면적은 " <<area<< "\n"; //면적을 출력하고 한 줄 띈다.
```  

___
### 2.4 키보드로 문자열 입력
* **C++의 문자열**
   * C-스트링 : C언어에서 문자열을 표현하는 방법
   * string : 문자열을 객체로 다루는 방법, 권장 방법


* **첫 번째 방법 : C-스트링 (1)** 
   *  널 문자('\0' 혹은 0)로 끝나는 char 배열을 말하며, C언어에서 사용하는 문자열 방식임
   *  C-스트링을 다루기 위해서는 #include <cstring> 또는 #include <string.h>를 사용해야 함
```C++
char name1[6] = { 'G', 'r', 'a', 'c', 'e', '\0' }; //name1은 문자열 "Grace"
char name2[5] = { 'G', 'r', 'a', 'c', 'e' }; //name2는 문자열이 아님, 단순 문자 배열
char name4[] = "Grace" //name4[] 배열의 크기는 6으로 자동 설정
```
```C++
//C-스트링을 이용하여 암호가 입력되면 프로그램을 종료하는 예
#include <iostream>
#include <cstring> // strcmp() 함수를 사용하기 위한 헤더 파일
using namespace std;

int main(){
  char password[11];
  cout << "프로그램을 종료하려면 암호를 입력하세요." << endl;
  while(true) {
    cout << "암호>>";
    cin >> password;
    if(strcmp(password, "C++") == 0) {
      cout << "프로그램을 정상 종료합니다." << endl;
      break;
    }
    else
      cout << "암호가 틀립니다~~" << endl;
  }
}

<실행 결과>
프로그램을 종료하려면 암호를 입력하세요.
암호>>Java
암호가 틀립니다~~
암호>>C++
프로그램을 정상 종료합니다.
```
* **첫 번째 방법 : C-스트링 (2)**
   * cin과 >> 연산자로 문자열을 입력 받을 때의 허점
     * 공백 문자를 만나면 그 전까지 입력된 문자들을 하나의 문자열로 인식함
   * cin.getline()을 이용하여 공백이 포함된 문자열 입력
     * cin 객체의 getline() 멤버 함수를 이용하면 공백이 포함된 문자열을 입력받을 수 있음
     * getline() 함수의 원형
```
cin.getline(char buf[]. int size, char delimitchar)
 - buf : 키보드로부터 읽은 문자열을 저장할 배열
 - size : buf[] 배열의 크기
 - delimitchar : 문자열 입력 끝을 지정하는 구분 문자
```
```C++
//cin.getline()을 이용한 문자열 입력
#include <iostream>
using namespace std;

int main() {
  cout << "주소를 입력하세요>>";

  char address[100];
  cin.getline(address, 100, '\n'); //키보드로부터 주소 읽기

  cout << "주소는 " << address << "입니다.\n"; //주소 출력
}
<실행 결과>
주소를 입력하세요>>컴퓨터시 프로그램구 C++동 스트링 1-1
주소는 컴퓨터시 프로그램구 C++동 스트링 1-1입니다
```

* **두 번째 방법 : string 클래스**
   * C-스트링은 배열의 크기에 의해 문자열의 크기가 고정되는 불편함이 있다면, string 클래스는 문자열의 크기에 제약이 없기 때문에 용이함
   * 객체 지향적일 뿐 아니라, C-스트링 방식보다 문자열을 다루기 쉬움
   * 예시 코드
```C++
//string 클래스를 이용한 문자열 입력 및 다루기
#include <iostream>
#include <string>
using namespace std;

int main() {
  string song("Falling in love with you"); //문자열 song
  string elvis("Elvis Presley"); //문자열 elvis
  string singer; //문자열 singer

  cout << song + "를 부른 가수는"; // +로 문자열 연결
  cout << "(힌트 : 첫글자는 " << elvis[0] << ")?"; //[] 연산자 사용

  getline(cin, singer); //문자열 입력
  if(singer == elvis) //문자열 비교
    cout << "맞았습니다.";
  else
    cout << "틀렸습니다. " + elvis + "입니다." << endl; //+로 문자열 연결
}

<실행 결과>
Falling in love with you를 부른 가수는(힌트 :  첫글자는 E)?Elvis Pride
틀렸습니다. Elvis Presley입니다.
``` 

___
### 2.3 #include <iostream>에 숨은 진실
