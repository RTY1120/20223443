## Server
```csharp

#define _WINSOCK_DEPRECATED_NO_WARNINGS
#include <winsock2.h>
#include <iostream>
#include <fstream>
#include <string>
#include <filesystem> // 파일목록 가져오기
#include <sstream>
#pragma comment(lib, "ws2_32.lib")

int main() {
    WSADATA wsaData;
    WSAStartup(MAKEWORD(2, 2), &wsaData); // WinSock 초기화

    SOCKET serverSock = socket(AF_INET, SOCK_STREAM, 0); // TCP 소켓 생성

    sockaddr_in serverAddr;
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_addr.s_addr = INADDR_ANY;  // 모든 IP에서 접속 허용
    serverAddr.sin_port = htons(9000);        // 포트 번호 9000 사용

    // 소켓에 주소 바인딩
    bind(serverSock, (SOCKADDR*)&serverAddr, sizeof(serverAddr));

    // 클라이언트 접속 대기
    listen(serverSock, 1);
    std::cout << "[서버] 클라이언트 접속 대기 중...\n";

    sockaddr_in clientAddr;
    int clientAddrSize = sizeof(clientAddr);
    SOCKET clientSock = accept(serverSock, (SOCKADDR*)&clientAddr, &clientAddrSize);
    std::cout << "[서버] 클라이언트 접속!\n";

    char buffer[1024];
    recv(clientSock, buffer, sizeof(buffer), 0);

    // 입력받은 문자가 list인지 확인 후 맞으면 파일목록은 전송, 아니면 none을 전송 후 프로그램 종료
    if (!strncmp("list", buffer, 4)) {
        std::string fileList = "a.txt, b.txt, c.png, d.png\n";
        send(clientSock, fileList.c_str(), (int)fileList.length(), 0);
    }
    else {
        std::cout << "none\n";
        closesocket(clientSock);
        closesocket(serverSock);
        WSACleanup();
        return 1;
    }
    
    // 버퍼 초기화
    memset(buffer, 0, sizeof(buffer));
    recv(clientSock, buffer, sizeof(buffer), 0);

    // 앞 4글자가 get 인지 확인
    if (!strncmp("get ", buffer, 4)) {
        // get뒤의 파일명 확인
        std::string filename(buffer + 4);

        // 보낼파일 열기
        std::ifstream file(filename, std::ios::binary);
        if (!file) {
            std::cerr << "[서버] " << filename << " 파일을 열 수 없습니다.\n";
            closesocket(clientSock);
            closesocket(serverSock);
            WSACleanup();
            return 1;
        }
        // 파일 내용을 읽어서 클라이언트로 전송
        while (!file.eof()) {
            file.read(buffer, sizeof(buffer));
            int bytesRead = file.gcount();
            send(clientSock, buffer, bytesRead, 0);
        }
        file.close();
        std::cout << "[서버] 파일 전송 완료!\n";
    }
    else {
        std::cout << "[클라이언트] get 형식이 아닙니다.";
    }


    // 정리
    closesocket(clientSock);
    closesocket(serverSock);
    WSACleanup();

    return 0;
}
```

## Client
```csharp

#define _WINSOCK_DEPRECATED_NO_WARNINGS
#include <winsock2.h>
#include <iostream>
#include <fstream>
#include <string>
#pragma comment(lib, "ws2_32.lib")

int main() {
    WSADATA wsaData;
    WSAStartup(MAKEWORD(2, 2), &wsaData); // WinSock 초기화

    SOCKET clientSock = socket(AF_INET, SOCK_STREAM, 0); // TCP 소켓 생성

    sockaddr_in serverAddr;
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_addr.s_addr = inet_addr("127.0.0.1"); // 서버 주소 (로컬)
    serverAddr.sin_port = htons(9000);                   // 서버 포트

    // 서버에 연결 시도
    if (connect(clientSock, (SOCKADDR*)&serverAddr, sizeof(serverAddr)) == SOCKET_ERROR) {
        std::cerr << "[클라이언트] 서버에 연결할 수 없습니다.\n";
        closesocket(clientSock);
        WSACleanup();
        return 1;
    }

    std::cout << "[클라이언트] 서버에 연결되었습니다.\n";

    // 키보드로부터 입력을 받아 서버에 전송
    std::string list;
    std::cout << "파일목록을 받기 위해 list를 입력하세요: ";
    std::getline(std::cin, list);
    send(clientSock, list.c_str(), (int)list.length(), 0);

    // 파일목록 출력
    char buffer[1024];
    int bytesReceived;
    bytesReceived = recv(clientSock, buffer, sizeof(buffer) - 1, 0);
    // 수신한 데이터 뒤에 \n 추가
    buffer[bytesReceived] = '\0';
    std::cout << "파일 목록: " << buffer << "\n";


    // get 파일명 전송
    std::string filename;
    std::cout << "get 파일명을 입력하세요: ";
    std::getline(std::cin, filename);
    send(clientSock, filename.c_str(), (int)filename.length(), 0);

    if (!strncmp("get ", filename.c_str(), 4)) {
        // get을 제외한 파일명, substr 문자열을 잘라서 반환
        std::string filename2 = filename.substr(4)
            ;
        // 수신한 데이터를 파일로 저장
        std::ofstream outFile(filename2, std::ios::binary);

        while ((bytesReceived = recv(clientSock, buffer, sizeof(buffer), 0)) > 0) {
            outFile.write(buffer, bytesReceived);
        }

        std::cout << "[클라이언트] 파일 수신 완료!\n";
        outFile.close();
    }
    else {
        std::cout << "[클라이언트] get 형식이 아닙니다.";
    }

    // 정리
    closesocket(clientSock);
    WSACleanup();

    return 0;
}
```
