## 20191128 이재경

## 비효율적인 코드가 수정된 클라이언트 코드
``` c

#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#define BUF_LEN 128

int main(int argc, char *argv[]){
	int s, n, len_in, len_out;
	struct sockaddr_in server_addr;

	char buf[BUF_LEN+1]	;
	
	if(argc != 3){
		printf("usage: %s ip_address + port number\n", argv[0]);
		exit(0);
	}
	
	// (char * haddr;)과 20라인(haddr = argv[1];)은 포인터로 28라인의 s.addr에 argv[1]의 값을 넣을것이기 때문에 포인터를 사용하지 않아도됨
	if((s = socket(PF_INET, SOCK_STREAM, 0)) < 0){
		printf("Cant't create socket\n");
		exit(0);
	}
	
	bzero((char *)&server_addr, sizeof(server_addr));
	server_addr.sin_family = AF_INET;
	server_addr.sin_addr.s_addr = inet_addr(argv[1]); //inet_addr의 인자를 haddr에서 argv[1]로 수정
	server_addr.sin_port = htons(atoi(argv[2])); //서버의 포트번호를 인자로 입력하기 위해 포트번호를 atoi(argv[2])로 수정
	
	if(connect(s, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0){
		printf("Can't connect.\n");
		exit(0);
	}
	
	printf("Input any string : ");
	if(fgets(buf, BUF_LEN, stdin)) {
		buf[BUF_LEN] = '\0';
		len_out = strlen(buf);
	} else {
		printf("fgets error\n");
		exit(0);
	}
	
	if (send(s, buf, len_out, 0) < 0) {
		printf("write error\n");
		exit(0);
	}
	
	printf("Echoed string : ");
	for(len_in=0, n=0; len_in < len_out; len_in += n){
		if((n = recv(s, &buf[len_in], len_out - len_in, 0)) < 0){
			printf("read error\n");
			exit(0);
		}
	}
	printf("%s", buf);
	close(s);
}
}
```
### 수정내용
* (char * haddr;)과 20라인(haddr = argv[1];)은 포인터로 28라인의 s.addr에 argv[1]의 값을 넣을것이기 때문에 포인터를 사용하지 않아도됨
* inet_addr의 인자를 haddr에서 argv[1]로 수정
* 서버의 포트번호를 인자로 입력하기 위해 포트번호를 atoi(argv[2])로 수정

## 비효율적인 코드가 수정된 클라이언트 코드
``` c

#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <stdlib.h>
#define BUF_LEN 128

int main(int argc, char *argv[]) {
	struct sockaddr_in server_addr, client_addr;
	int server_fd, client_fd;   
	int len, msg_size;
	char buf[BUF_LEN+1];
	
	if(argc !=3){
		printf("usage: %s port+address\n", argv[0]);
		exit(0);
	}
	
	if((server_fd = socket(PF_INET, SOCK_STREAM, 0)) < 0){
		printf("Server: Can't open stream socket.");
		exit(0);
	}
	
	bzero((char *)&server_addr, sizeof(server_addr));
	server_addr.sin_family = AF_INET;
	server_addr.sin_addr.s_addr = inet_addr(argv[2]); //INADDR_ANY대신 서버의 주소를 인자로 입력하게 수정
	server_addr.sin_port = htons(atoi(argv[1]));
	
	if(bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0){
		printf("Server: Can't bind local address.\n");
		exit(0);
	}
	
	listen(server_fd, 5);
	
	len = sizeof(client_addr); //len = sizeof(client_addr);//client_addr while(1)안에서 값에서 변하지 않기 때문에 반복문 밖으로 수정
	
	while(1) {
		printf("Server : waiting connection request.\n");

		client_fd = accept(server_fd, (struct sockaddr *)&client_addr, &len);
		if(client_fd < 0) {
			printf("Server: accept failed.\n");
			continue; //accept를 실패해도 끝내지 않고 계속 돌아가게 exit(0)에서 continue로 수정
		}
		
		printf("Server : A client connected.\n");
		msg_size = recv(client_fd, buf, sizeof(buf), 0);
		send(client_fd, buf, msg_size, 0);
		close(client_fd);		
	}
	close(server_fd);
}
```
### 수정내용
*  INADDR_ANY대신 서버의 주소를 인자로 입력하게 수정
*  len = sizeof(client_addr);//client_addr while(1)안에서 값에서 변하지 않기 때문에 반복문 밖으로 수정
*  accept를 실패해도 끝내지 않고 계속 돌아가게 exit(0)에서 continue로 수정

### 클라이언트 실행화면
![image](https://user-images.githubusercontent.com/50912987/229797521-1a46ff1a-37df-471b-90c2-9324aab87ec6.png)

### 서버 실행화면
![image](https://user-images.githubusercontent.com/50912987/229797737-8d5b1177-3659-410a-9de1-69b28d6ff8c2.png)


