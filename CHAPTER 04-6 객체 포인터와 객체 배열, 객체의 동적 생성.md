# string 클래스를 이용한 문자열 사용

<details>
<summary>1. string 클래스 개요</summary>
<div markdown="1">       

* string 클래스는 문자열의 크기에 맞추어 스스로 메모리 크기를 조절하므로 사용하기 매우 편리함
  >string str = “I Love “;  //str은 ‘I’, ‘ ‘, ‘l’, ‘o’, ‘v’, ‘e’, ‘ ‘ 의 7개 문자로 구성
  >
  >str.append(”C++.”);  //str은 “I love C++.”이 된다.
</div>
</details>

___

<details>
<summary>2. string 객체 생성 및 출력</summary>
<div markdown="1">       

#### string 객체 생성
* 다양한 방법을 통해 문자열 객체를 생성할 수 있으며, 문자열의 크기에는 제한이 없음
```C++
string str;  //빈 문자열을 가진 스트링 객체
string address(”서울시 성북구 삼선동 389”);  //문자열 리터럴로 초기화
string copyAddress(address);  //address를 복사한 copyAddress 생성

//C-스트링(char [] 배열)으로부터 스트링 객체 생성
char text[] = { ’L’, ‘o’, ‘v’, ‘e’, ‘ ‘, ‘C’, ‘+’, ‘+’, ‘\0’ };  //C-스트링
string title(text);  //”Love C++”을 가진 string 객체 생성
```

#### string 객체가 가진 문자열 출력(cout<<을 이용하여 쉽게 화면에 출력 가능)
```C++
cout << address << endl;  //”서울시 성북구 삼선동 389” cnffur
cout << title << endl;  //”Love C++” 출력
```

#### string 객체의 동적 생성
* `new`와 `delete` 연산자를 이용하여 동적으로 생성하고 반환할 수 있음
```C++
string *p = new string(”C++”);  //스트링 객체 동적 생성
cout << *p;  //”C++” 출력
p→append(” Great!!”);  //p가 가리키는 스트링이 “C++ Great!!”가 됨
cout << *p;  //”C++ Great!!” 출력
delete p;  //스트링 객체 반환
```

#### string 클래스를 이용한 문자열 생성 및 출력 예시 코드
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	//스트링 생성
	string str;  //빈 문자열을 가진 스트링 객체 생성
	string address("서울시 성북구 삼선동 389");
	string copyAddress(address); //address의 문자열을 복사한 스트링 객체 생성

	char text[] = { 'L', 'o', 'v','e', ' ', 'C', ' + ',' + ', '\0' }; //C-스트링
	string title(text); //"Love C++" 문자열을 가진 스트링 객체 생성

	//스트링 출력
	cout << str << endl;  //빈 스트링. 아무 값도 출력되지 않음
	cout << address << endl;
	cout << copyAddress << endl;
	cout << title << endl;
}

<실행 결과>

서울시 성북구 삼선동 389
서울시 성북구 삼선동 389
Love C++
```
</div>
</details>

___

<details>
<summary>3. string 객체에 문자열 입력</summary>
<div markdown="1">       

#### string 객체에 문자열 입력
* `>> 연산자`를 사용하여 입력하기 쉽지만, 공백 문자가 입력되면 그 앞까지 하나의 문자열로 다루기 때문에 **공백 문자를 포함하는 문자열은 읽어 들일 수 없음**
* 이런 문제를 해결하기 위해 `getline() 전역 함수`를 이용하면 됨
 >string name;
 >
 >getline(cin, name, ‘\n’);  // ’\n’을 만날 때까지 키보드(cin)로부터 문자열을 읽어 name에 저장

#### string 배열과 문자열 키 입력 응용
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string names[5]; //string 배열 선언

	for (int i = 0; i < 5; i++) {
		cout << "이름 >> ";
		getline(cin, names[i], '\n');
	}

	string latter = names[0];
	for (int i = 1; i < 5; i++) {
		if (latter < names[i]) {  //names[i]가 latter보다 뒤에 온다면
			latter = names[i]; //latter 문자열 변경
		}
	}

	cout << "사전에서 가장 뒤에 나오는 문자열은 " << latter << endl;
}
```
</div>
</details>

___

<details>
<summary>4. 문자열 다루기</summary>
<div markdown="1">       

#### 문자열 치환
* `= 연산자` 이용
> string a = “Jave”, b = “C++”;
> 
> 
> **a = b;**  // a = “C++”이 됨. a는 b를 복사한 문자열을 가짐
>

#### 문자열 비교
* `compare() 함수` 이용, 같으면 0, str보다 앞이면 -, 뒤면 + 리턴
* 비교 연산자를 이용하면 좀 더 효율적으로 구현 가능함
> string name = “Kitae”;
> 
> 
> string alias = “Kito”;
> 
> int res = **name.compare(alias);**  //name과 alias를 비교한다.
> 
> if(res == 0) cout << “두 문자열이 같다.”;  //name과 alias가 동일
> 
> else if(res < 0) cout << name << “ < “ << alias << endl;  //name이 앞에 옴
> 
> else cout << alias << “ < “ << name << endl;  //name이 뒤에 옴
> 

> if(name == alias) cout << “두 문자열이 같다”;
> 
> 
> if(name < alias) cout << name << “이 “ <<alias << “보다 사전에서 먼저 나온다”;
>

#### 문자열 연결
* `append() 함수` 이용, `+, += 연산자`를 이용하면 쉽게 작성 가능
> string a(”I”);
> 
> 
> **a.append**(” love “);  // a = “I love “
> 

> string a(”I love C++”);
> 
> 
> string b(”.”);
> 
> string c;
> 
> c = a **+** b;  //a, b문자열에는 변화가 없고, c = “I love C++.”로 변경됨
> 
> c**+=** b;  // b문자열에는 변화가 없고, c = “I love C++..”로 변경됨
>

#### 문자열 삽입
* `insert() 함수`, `replace() 함수` 이용
* 문자열에 새로운 문자열이나 문자의 삽입 삭제가 가능하며,
* 문자열의 일부분을 다른 문자들로 변경할 수 있음
> string a(”I love C++”);
> 
> 
> a.insert(2, “really “);  //a - “I really love C++”
> 
> a.replace(2, 11, “study”);  // a = “I study C++”
>

#### 문자열 길이
* `length()` 와 `size() 함수`는 문자열의 길이를 리턴함
* 길이와는 달리 string 객체의 내부 메모리 용량을 리턴하는 `capacity()` 도 있음
> string a(”I study C++”);
> 
> 
> int length = a.length();  //”I study C++”의 문자 개수는 11이다. length = 11
> 
> int size = a.size();  //length()와 동일하게 작동. size = 11
> 
> int capactiy = a.**capacity()**;  //스트링 a의 현재 용량 capacity = 31, 변할 수 있다.
>

#### 문자열 삭제
* `erase()` 는 문자열의 일부분을 삭제, `clear()` 는 완전히 삭제함
> string a(”I love C++”);
> 
> 
> a.erase(0, 7);  //a의 처음부터 7개의 문자 삭제. a = “C++”로 변경
> 
> a.clear();  // a = “”
>

#### 서브스트링
* `substr() 함수`를 사용하여 문자열에서 일부분을 발췌한 문자열을 얻을 수 있음
> string b = “I love C++”;
> 
> 
> string c = b.substr(2, 4);  //b의 인덱스 2에서 4개의 문자 리턴. c = “love”
> 
> string d = b.substr(2);  //b의 인덱스 2에서 끝까지 문자열 리턴. d = “love C++”
>

#### 문자열 검색
* 특정 문자열이 존재하는지 검색하는 기능, `find()` 이용
* 특정 문자나 문자열을 발견하면 첫 번째 인덱스를 리턴, 발견하지 못하면 -1 리턴
> string e = “I love love C++”;
> 
> 
> int index = e.find(”love”);  //e에서 “love” 검색. 인덱스 2 리턴
> 
> index = e.find((”love”, index+1);  //e의 인덱스 3부터 “love” 검색. 인덱스 7 리턴
> 
> index = e.find((”C#”); //e에서 “C#”을 발견할 수 없음. -1 리턴
> 
> index = e.find((’v’, 7) //e의 인덱스 7부터 ‘v’ 문자 검색. 인덱스 9 리턴
>

#### 문자열의 각 문자 다루기
* `at() 함수`와 `[] 연산자` 둘 다 문자열의 특정 위치에 있는 문자를 리턴
> string f(”I love C++”);
> 
> 
> char ch1 = f.at(7);  //문자열 f의 인덱스 7에 있는 문자 리턴. ch1 = ‘C’
> 
> char ch2 = f[7];  // f.at(7)과 동일한 표현. ch2 = ‘C’
> 
> f[7] = ‘D’;  //f는 “I love D++”
> 
> char ch3 = f.at(f.length()-1);  //ch3은 ‘+’
>
</div>
</details>

___

<details>
<summary>5. 문자열의 숫자 변환, stoi()</summary>
<div markdown="1">       

* 문자열을 숫자로 변환하는 전역 함수 `stoi()` 를 추가
> string year = “2014”;
> 
> 
> int n = **stoi**(year);  //n은 정수 2014 값을 가짐
> 
> // int n = atoi(year.c_str());  //비주얼 C++ 2008 이하
>
</div>
</details>

___

<details>
<summary>6. 문자 다루기</summary>
<div markdown="1">       

* `toupper()` , `isdigit()` , `isalpha()` 함수를 사용하는 예
> string a = “hello”;
> 
> 
> for(int i=0; i<a.length(); i++) a[i] = toupper(a[i]); //a가 “HELLO”로 변경
> 
> cout << a; //”HELLO” 출력
> 
> if(isdigit(a[0])) cout << “숫자”;
> 
> else if(isalpha(a.at(0))) cout << “문자”; //a[0]은 문자 ‘H’
>
</div>
</details>

___

<details>
<summary>7. 문자열 관련 예시 코드</summary>
<div markdown="1">       

#### 문자열을 입력 받고 회전시키는 예시
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string s;

	cout << "아래에 문자열을 입력하세요. 빈 칸이 있어도 됩니다.(한글 안됨) " << endl;
	getline(cin, s, '\n'); //문자열 입력
	int len = s.length(); //문자열의 길이

	for (int i = 0; i < len; i++) {
		string first = s.substr(0, 1); //맨 앞의 문자 1개를 문자열로 분리
		string sub = s.substr(1, len - 1); //나머지 문자들을 문자열로 분리
		s = sub + first; //두 문자열을 연결하여 새로운 문자열로 만듦
		cout << s << endl;
	}
}

<실행 결과>
아래에 문자열을 입력하세요. 빈 칸이 있어도 됩니다.(한글 안됨)
I love you
 love youI
love youI
ove youI l
ve youI lo
e youI lov
 youI love
youI love
ouI love y
uI love yo
I love you
```

#### 문자열 처리 응용 - 덧셈 문자열을 입력받아 덧셈 실행 예시
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string s;
	cout << "7+23+5+100+25와 같이 덧셈 문자열을 입력하세요." << endl;
	getline(cin, s, '\n'); //문자열 입력
	int sum = 0;
	int startIndex = 0; //문자열 내에 검색할 시작 인덱스
	while (true) {
		int fIndex = s.find('+', startIndex); // '+' 문자 검색
		if (fIndex == -1) { // '+' 문자 발견할 수 없음
			string part = s.substr(startIndex);
			if (part == "") break; //"2+3+", 즉 +로 끝나는 경우
			cout << part << endl;
			sum += stoi(part); //문자열을 수로 변환하여 더하기
			break;
		}
		int count = fIndex - startIndex; //서브스트링으로 자를 문자 개수
		string part = s.substr(startIndex, count); //startIndex부터 count 개의 문자로 서브스트링 만들기

		cout << part << endl;
		sum += stoi(part); //문자열을 수로 변환하여 더하기
		startIndex = fIndex + 1; //검색을 시작할 인덱스 전진시킴
	}
	cout << "숫자들의 합은 " << sum;
}

<실행 결과>
7+23+5+100+25와 같이 덧셈 문자열을 입력하세요.
66+2+8+55+100
66
2
8
55
100
숫자들의 합은 231
```

#### 문자열 find 및 replace 예시
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string s;
	cout << "여러 줄의 문자열을 입력하세요. 입력의 끝은 &문자입니다." << endl;
	getline(cin, s, '&'); //문자열 입력
	cin.ignore(); // '&' 뒤에 따라 오는 <Enter> 키를 제거하기 위한 코드!

	string f, r;
	cout << endl << "find: ";
	getline(cin, f, '\n'); //검색할 문자열 입력
	cout << "replace: ";
	getline(cin, r, '\n'); //대치할 문자열 입력

	int startIndex = 0;
	while (true) {
		int fIndex = s.find(f, startIndex); //startIndex부터 문자열 f 검색
		if (fIndex == -1)
			break; //문자열 s의 끝까지 변경하였음
		s.replace(fIndex, f.length(), r); //fIndex부터 문자열 f의 길이만큼 문자열 r로 변경
		startIndex = fIndex + r.length();
	}
	cout << s << endl;
}

<실행 결과>
여러 줄의 문자열을 입력하세요. 입력의 끝은 &문자입니다.
It's now or never, come hold me tight. Kiss me my darling, be mine tonight 
Tomorrow will be too late. It's now or never, my love won't wait&

find: now
replace: Right Now
It's Right Now or never, come hold me tight. Kiss me my darling, be mine tonight 
Tomorrow will be too late. It's Right Now or never, my love won't wait
```
</div>
</details>
