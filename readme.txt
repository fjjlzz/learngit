#include<stdio.h>
#include<winsock2.h>
#include<process.h>
#include<windows.h>
#pragma comment(lib,"ws2_32.lib")
char recdata[2500];
unsigned _stdcall recvThreadfun(void* pArgumentsl)
{
	while (true)
	{
		int ret = recv((SOCKET)pArgumentsl, recdata, 255, 0);
		if (ret > 0)
		{
			recdata[ret] = '\0';
			printf("收到服务器数据：%s\n", recdata);
		}
	}
}
int main(int argc, char* argv[])
{
	WORD sockVersion = MAKEWORD(2, 2);
	WSADATA data;
	if (WSAStartup(sockVersion, &data) != 0)
	{
		return 0;
	}
	SOCKET socket_Server = (AF_INET, SOCK_STREAM, 0);
	if (socket_Server == INVALID_SOCKET)
	{
		printf("invalid socket");
		return 0;
	}
	SOCKADDR_IN Server_add;
	Server_add.sin_family = AF_INET;
	Server_add.sin_addr.S_un.S_addr = htonl(192.168.73.1);
	Server_add.sin_port = htonl(6666);
	bind(socket_Server, (SOCKADDR*)&Server_add, sizeof(SOCKADDR));
	if (connect(socket_Server, (SOCKADDR*)&Server_add, sizeof(SOCKADDR) = INVALID_SOCKET)
	{
		printf("connect error");
		closesocket(socket_Server);
		return 0;
	}
	unsigned xc;
	xc = benginThread(NULL, 0, recvThreadfun, NULL, 0, NULL);
	printf("开始接受服务器数据");
	char senddata[2500];
	while (true)
	{
		printf("输入想要发送的数据：\n");
		gets(senddata);
		send(client.senddata, strlen(senddata), 0);
	}
	return 0;
}