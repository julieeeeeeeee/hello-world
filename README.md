# hello-world

#include<stdlib.h>
#include<stdio.h>
#include<string.h>
#include<sys/types.h>
#include<sys/wait.h>
#include<unistd.h>
//创建子进程fork（）是简单的拷贝，而vfork（）类似于线程，强制执行子进程让父进程阻塞，在子进程执行exit前子进程共享父进程的数据，
//子进程结束后再运行父进程。
int main()
{
	pid_t child;  //定义子进程ID，相当于int
	int i=0;
	printf("i = %d\n",i);
	char str[50];
	puts("1.excute");
	child = vfork();
	//child = fork();
//无论父子进程都是从fork或vfork后运行的，fork创建了子进程后，在子进程中返回0，在父进程中返回子进程的pid。
 
	puts("2.excute");
	
	if(child== -1)//如果创建进程失败
	{
		perror("fork");
		puts("fail to create a sup process");
		//exit(1);
	}
	else if(child == 0)//进程创建成功
	{
		puts("\nin child:");
		printf("\tchild pid = %d\n",getpid());
		printf("\tchild ppid = %d\n",getppid());
		puts("writing in file :\n This is my work\n");
		FILE  *fp = fopen("123.txt","w");//新写一个文件
		fprintf(fp,"%s","This is my work");//在文件中写入内容
		i++;
		printf("i = %d\n",i);
		fclose(fp);
		exit(0);//终止子进程
	}
	else
	{
		waitpid(child,NULL,0);
//等待pid为child的子进程运行，当子进程运行完后父进程运行。
		puts("\nin parent:");
		puts("Reading from file ...");
		FILE *fp = fopen("123.txt","r");
		fgets(str,50,fp);
		puts(str);
		fclose(fp);
		i++;
		printf("i = %d\n",i);
		//exit(0);
	}
	return 0;
}
