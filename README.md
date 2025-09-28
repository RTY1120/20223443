# 20223443
## 객체지향 프로그래밍 4주차
### 상속

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


