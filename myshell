#include <stdio.h>

#include <string.h>

#include <stdbool.h>

#include <stdlib.h>

#include <unistd.h>

#include <sys/types.h>

#include <sys/wait.h>

#include <pwd.h>

#define COLOR_GREEN "\x1b[32m" //그린 색상

#define COLOR_BLUE "\x1b[34m" //블루 색상


#define COLOR_RED "\x1b[31m" //빨간 색상

#define COLOR_RESET "\x1b[0m" //초기화 색

#define MAX_LEN_LINE    50

#define LEN_HOSTNAME	30

#define _CRT_SECURE_NO_WARNINGS

 

int main(void)

{

    char command[MAX_LEN_LINE];

    char *args[] = {command, NULL};

    int ret, status;

    pid_t pid, cpid;

    

    while (true) {

        char *s;

        int len;

        char *prog[10] = {NULL, };  // 각각의 프로그램 

	int prog_count = 0;    //실행되는 프로그램 

	uid_t user_id;

	struct passwd *pw_name;

 

	char hostname[LEN_HOSTNAME + 1];  //  hostname을 초기화한다. 

	memset(hostname, 0x00, sizeof(hostname));

	gethostname(hostname, LEN_HOSTNAME);




	printf(COLOR_GREEN "%s@%s ~~~~$ " COLOR_RESET, getpwuid(getuid())->pw_name, hostname); //  콘솔 색상 그린으로 변경

        

	// stdin 위치에서 첫번째 개행 또는 MAX_LEN_LINE만큼 string을 복사하여 command에 저장한다.

        s = fgets(command, MAX_LEN_LINE, stdin);

        len = strlen(command);

        printf("%d\n", len);

        if (command[len - 1] == '\n') {

            	command[len - 1] = '\0'; 

       	}

        if (s == NULL) {

            fprintf(stderr, "fgets failed\n");

            exit(1);

        }

	else if (!strcmp(s, "exit")) {   //입력값을 "exit"과 비교한다.

	    printf(COLOR_RED "-----Shell Terminated-----\n" COLOR_RESET);  //입력값에 "exit"이 있을경우 

	    break;  // while을 빠져나온다. 

	}
        
    

	else if (strstr(s, "; ") != NULL) {  // 입력값중에 ;이 포함되었을 경우

		int i = 0;

		char *ptr = strtok(s, "; ");  //입력값을 ;을 기준으로 자른 토큰을 반환한다.

		while(ptr != NULL) {    //NULL이 나올때 까지 반복한다.

			prog[i] = ptr;  //prog안에 잘라진 토큰을 한개씩 넣는다.

                        i++;

			prog_count +=1 ;

			ptr = strtok(NULL, "; ");
                         
                       

		}

	}

	else {

		prog_count += 1;

		prog[0] = command;

	}

 


	printf(COLOR_BLUE "%d program processing\n\n" COLOR_RESET, prog_count); // 실행되는 프로그램 수를 출력해준다.

 

      

	for(int i=0;i<prog_count;i++) {

		args[0] = prog[i];

		printf("[%s]\n", args[0]);

		pid = fork();

		if (pid < 0) {

		    fprintf(stderr, "fork failed\n");

		    exit(1);

		} 

		if (pid != 0) {   /* parent */ 

		    cpid = waitpid(pid, &status, 0);

		    if (cpid != pid) {

			fprintf(stderr, "waitpid failed\n");        

		    }

		    printf("Child process terminated\n");

		    if (WIFEXITED(status)) {

			printf("Exit status is %d\n\n\n", WEXITSTATUS(status)); 

		    }

		}

		else {   /* child */

		    ret = execve(args[0], args, NULL);

		    if (ret < 0) {
                        printf(COLOR_RED "program dosen't not existed\n\n" COLOR_RESET);

			fprintf(stderr, "execve failed\n");   

			return 1;

		    }

		}

	}

    }

    return 0;

}
