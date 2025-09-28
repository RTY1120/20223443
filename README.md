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
	void countPerson()
	{
		cout << getPerson() << endl;	// 3. 부모 클래스 Vehicle의 탑승 인원을 확인하는 getPerson()함수 사용 가능
	}
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
	void countPerson()    // 탑승인원 확인
	{
		cout << getPerson() << endl;
	}
};

int main(int argc, char const* argv[])
{
	Cruise dolphin;
	dolphin.ride();
	dolphin.load(10);
	dolphin.countPerson();

	AirPlane cppAir;
	cppAir.ride();
	cppAir.load(20);
	cppAir.countPerson();
}


```


