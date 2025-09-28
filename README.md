# 20223443
## 객체지향 프로그래밍 4주차
### 4주차

#### 1번
```csharp

#include <iostream>
using namespace std;

class Vehicle
{
private:
	int person = 0;    // 탑승인원
	int baggage = 0;       // 화물 무게
public:
	void ride()  // 탑승
	{
		person++;
	}
	void load(int weight)
	{
		baggage += weight;		// weight를 baggage에 추가
	};   // 짐 싣기
	void getOff()
	{
		person--;
	};   // 하차
	int getPerson()  // 탑승인원 확인
	{
		return person;
	}
	void countPerson() {
		cout << "사람: " << getPerson() << "명" << endl;	// 자식 클래스의 countPerson() 주석 처리 후 부모클래스에 선언
	}

	int getBaggage() {		// 화물무게 확인
		return baggage;
	}
};

class Cruise :public Vehicle		// 1. 부모 클래스 Vehicle로 부터 상속접근지정자 public으로 상속받는 자식 클래스 Cruise
{
private:
	int room;
public:
	void setRoom(int room)				// 2. 부모 클래스 Vehicle에는 없는 setRoom 함수 자식 클래스에서 선언
	{
		this->room = room;
	};    // 크루즈의 방 수 설정

	void getRoom() {
		cout << "방 개수: " <<  room << "개" << endl;
	}
	/*
	void countPerson()
	{
		cout << "사람: " << getPerson() << "명" << endl;	// 3. 부모 클래스 Vehicle의 탑승 인원을 확인하는 getPerson()함수 사용 가능
	}
	*/
};

class AirPlane :public Vehicle
{
private:
	int seat;
public:
	void setSeat(int seat)
	{
		this->seat = seat;
	}
	/*
	void countPerson()    // 탑승인원 확인
	{
		cout << "사람: " << getPerson() << "명" << endl;
	}
	*/
};

int main(int argc, char const* argv[])
{
	Cruise dolphin;
	dolphin.ride();		// 부모클래스 멤버함수 접근
	dolphin.load(10);	// 부모클래스 멤버함수 접근
	dolphin.countPerson();	// 자식클래스 멤버함수 호출
	dolphin.setRoom(5);	// 방 수 설정
	dolphin.getRoom();	// 방 수 출력


	AirPlane cppAir;
	cppAir.ride();		// 부모클래스의 멤버함수 접근
	cppAir.load(20);	// 부모클래스의 멤버함수 접근
	cppAir.countPerson();	// 자식클래스 멤버함수 호출
}

```

#### 2번

```csharp
#include <iostream>
using namespace std;

class Base {
private:
	int a = 1;		// Base 클래스에서만 접근 가능
public:
	int b = 2;		// 어디서나 접근가능 
protected:
	int c = 3;		// Base 클래스 내부와 상속 받은 자식 클래스에서 접근가능
};

class Drived1 : private Base {
public:
	void show() {
//		cout << a << endl;		// 부모클래스 Base의 private 멤버 변수이기 때문에 접근 불가능
		cout << b << endl;		// 접근 가능
		cout << c << endl;		//접근 가능
	}
};
class Drived2 : protected Base {
public:
	void show() {
//		cout << a << endl;	// 접근 불가능
		cout << b << endl;	// 접근 가능
		cout << c << endl;	// 접근 가능
	};
};

class Drived3 : public Base {
public:
	void show() {
//		cout << a << endl;	// 접근 불가능
		cout << b << endl;	// 접근 가능
		cout << c << endl;	// 접근 가능
	};
};

int main()
{
	Drived1 d1;
	Drived2 d2;
	Drived3 d3;
	d1.show();
	d2.show();			// cout << a << endl; 모두 주석 처리시 간접접근 오류발생하지 않음
	d3.show();
	d3.b = 10;		// 상속접근지정자가 public이기 때문에 직접 접근 가능
	//	d2.b = 10;		// 상속접근지정자가 protected이기 때문에 직접접근 불가능
	//	d1.b = 20;		// 상속접근지정자가 private이기 때문에 직접 접근 불가능
};

```

#### 3번. 과제 외에 프로그램 작성한 것

```csharp

#include <iostream>
using namespace std;

class Person {
	string name;
	int age;
public:
	Person() {};

	Person(int a, string b) {
		age = a;
		name = b;
	}
	void show() {
		cout << "이름:" << name << ", " << "나이:" << age << "살" << endl;
	}
};

class Student : public Person {		// 부모 클래스 Person이 자식 클래스 Student에 상속
	string major;
public:
	Student(int a, string b, string c) : Person(a, b) {		//초기화 리스트로 age, name 초기화
		major = c;
	}
	void introduce() {
		cout << "학과:" << major << endl;
	}
};

int main()
{
	Person p1(20, "가나다");
	p1.show();		// 출력: 이름:가나다, 나이:20
	Student p2(25, "마바사", "게임공학");
	p2.show();		// 출력: 이름:마바사, 나이:25, 부모클래스 Person으로부터 상속받은 show() 사용
	p2.introduce();	// 출력: 학과:게임공학
}


```


