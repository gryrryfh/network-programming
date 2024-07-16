```
/*----------------------------------------------------------------
myecho.c
gcc -o myecho myecho.c
gcc -o myecho myecho.c -lsocket -lnsl
 myecho 127.0.0.1
----------------------------------------------------------------*/
```
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
	char *haddr;
	char buf[BUF_LEN+1]	;
	
	if(argc != 2){
		printf("usage: %s ip_address\n", argv[0]);
		exit(0);
	}
	
	if((s = socket(PF_INET, SOCK_STREAM, 0)) < 0){
		printf("Cant't create socket\n");
		exit(0);
	}
	/* echo ?쒕쾭???뚯폆二쇱냼 援ъ“泥??묒꽦 */
	bzero((char *)&server_addr, sizeof(server_addr));
	server_addr.sin_family = AF_INET;
	server_addr.sin_addr.s_addr = inet_addr(argv[1]]);
	server_addr.sin_port = htons(2049);
	
	/* ?곌껐?붿껌 */
	if(connect(s, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0){
		printf("Can't connect.\n");
		exit(0);
	}
	
	/* ?ㅻ낫???낅젰??諛쏆쓬 */
	printf("Input any string : ");
	if(fgets(buf, BUF_LEN, stdin)) {
		buf[BUF_LEN] = '\0';
		len_out = strlen(buf);
	} else {
		printf("fgets error\n");
		exit(0);
	}
	
	/* echo ?쒕쾭濡?硫붿떆吏 ?≪떊 */
	if (send(s, buf, len_out, 0) < 0) {
		printf("write error\n");
		exit(0);
	}
	
	/* ?섏떊??echo 硫붿떆吏 ?붾㈃ 異쒕젰 */
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
```
```
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
	
	if(argc !=2){
		printf("usage: %s port\n", argv[0]);
		exit(0);
	}

	if((server_fd = socket(PF_INET, SOCK_STREAM, 0)) < 0){
		printf("Server: Can't open stream socket.");
		exit(0);
	}
	
	bzero((char *)&server_addr, sizeof(server_addr));

	server_addr.sin_family = AF_INET;
	server_addr.sin_addr.s_addr = inet_addr("192.168.122.1"); //서버에 ip주소가 하나밖에 없으므로 // 
	server_addr.sin_port = htons(atoi(argv[1]));
	
	if(bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0){
		printf("Server: Can't bind local address.\n");
		exit(0);
	}
	
	listen(server_fd, 5);
	len = sizeof(client_addr); 
	
	while(1) {
		printf("Server : waiting connection request.\n");

					client_fd = accept(server_fd, (struct sockaddr *)&client_addr, &len);
		if(client_fd < 0) {
			printf("Server: accept failed.\n");
			continue;
		}
		
		printf("Server : A client connected.\n");
		msg_size = recv(client_fd, buf, sizeof(buf), 0);
		send(client_fd, buf, msg_size, 0);
		close(client_fd);		
	}
	close(server_fd);
}
```
	
