# students-info-manage-system
to manage the students' information
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <mem.h>

#define MAX_LENGTH_OF_NUMBER 13
#define MAX_LENGTH_OF_NAME 31
#define MAX_LENGTH_OF_EMAIL 63
#define MAX_NUM_OF_STUDENTS 10000

typedef struct Student{
	char number[MAX_LENGTH_OF_NUMBER+1];
	char name[MAX_LENGTH_OF_NAME+1];
	int chinese;
	int math;
	int english;
	char email[MAX_LENGTH_OF_EMAIL+1];
}Student;
typedef struct StudentDataBase{
	Student studentArray[MAX_NUM_OF_STUDENTS];
	int numOfStudents;
}StudentDataBase;
typedef enum{ERROR,OK} Status;
typedef enum{NO,YES} Bool;

void PrintStudent(Student* pStudent);//输出单个学生 
void PrintAllStudents(StudentDataBase* pStudentDataBase);//输出所有学生 
Status ReadStudentFile(const char* fileName,StudentDataBase* pStudentDataBase);//从文件读入学生信息 
Status InsertStudent(StudentDataBase* pSutndentDataBase);//插入学生信息 
void SortStudentsNumber(StudentDataBase* pStudentDataBase);//按学号排序 
void SortStudentsChineseDown(StudentDataBase* pStudentDataBase);//按语文成绩降序排序 
void SortStudentsMathDown(StudentDataBase* pStudentDataBase);//按数学成绩降序排序 
void SortStudentsEnglishDown(StudentDataBase* pStudentDataBase);//按英语成绩降序排序 
void SortStudentsTotalDown(StudentDataBase* pStudentDataBase);//按总分降序排序 
void SortStudentsChineseUp(StudentDataBase* pStudentDataBase);//按语文成绩升序排序 
void SortStudentsMathUp(StudentDataBase* pStudentDataBase);//按数学成绩升序排序 
void SortStudentsEnglishUp(StudentDataBase* pStudentDataBase);//按英语成绩升序排序 
void SortStudentsTotalUp(StudentDataBase* pStudentDataBase);//按总分升序排序 
void SortStudent(StudentDataBase* pStudentDataBase);//选择排序类型
void DeleteStudentByName(StudentDataBase* pStudentDataBase);//按姓名删除学生信息 
void DeleteStudentByNumber(StudentDataBase* pStudentDataBase);//按学号删除学生信息 
void DeleteStudent(StudentDataBase* pStudentDataBase);//选择删除方法 
Status PrinfFile(const char* flieName,StudentDataBase* pStudentDataBase);//将学生信息输出到文件 
int StatisticChinese(StudentDataBase* pStudentDataBase);//统计语文及格率和及格人数 
int StatisticMath(StudentDataBase* pStudentDataBase);//统计数学及格率和及格人数
int StatisticEnglish(StudentDataBase* pStudentDataBase);//统计英语及格率和及格人数
int StatisticTotal(StudentDataBase* pStudentDataBase);//统计三科都及格率和及格人数
void Statistic(StudentDataBase* pStudentDataBase);//选择统计方式 
float ChineseAverage(StudentDataBase* pStudentDataBase,float a);//计算语文平均成绩 
float MathAverage(StudentDataBase* pStudentDataBase,float a);//计算数学平均成绩 
float EnglishAverage(StudentDataBase* pStudentDataBase,float a);//计算英语平均成绩
float TotalAverage(StudentDataBase* pStudentDataBase,float a);//计算总分平均成绩
void Average(StudentDataBase* pStudentDataBase);//选择计算平均成绩方式 
void OperationSelect(const char* fileName,Student* pStudent,StudentDataBase* pStudentDataBase);//选择操作方式 
void OneStudent(StudentDataBase* pStudentDataBase);//对单个学生信息进行操作 
Status ReviseStudent(Student* pStudent);//选择替换学生信息方式 
void ReviseStudentChinese(Student* pStudent);//替换学生语文成绩 
void ReviseStudentMath(Student* pStudent);//替换学生数学成绩 
void ReviseStudentEnglish(Student* pStudent);//替换学生英语成绩 
void ReviseStudentAll(Student* pStudent);//替换完整学生信息 
Status DeleteOneStudent(StudentDataBase* pStudentDataBase,int a);//删除一个学生信息 
void SeekStudent(StudentDataBase* pStudentDatBase);//查找学生信息 
Bool IsNumberWrong(StudentDataBase* pStudentDataBase);//判断学号是否错误 
Bool IsScoreWrong(StudentDataBase* pStudentDataBase);// 判断分数是否错误 
Bool IsEmailWrong(StudentDataBase* pStudentDataBase);// 判断邮箱是否错误
Bool IsImformationWrong(StudentDataBase* pStudentDataBase);//判断信息是否错误 

void PrintStudent(Student* pStudent){//输出单个学生,参数为Student指针,无返回值 
	printf("%s\t%s\t%d\t%d\t%d\t%s\n",pStudent->number,pStudent->name,pStudent->chinese,pStudent->math,pStudent->english,pStudent->email);
	return;
}
void PrintAllStudents(StudentDataBase* pStudentDataBase){//输出所有学生,参数为StudentDataBase指针,无返回值 
	int i,n;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n;i++){
		PrintStudent(&(pStudentDataBase->studentArray[i]));
	}
	return;
}
Status ReadStudentFile(const char* fileName,StudentDataBase* pStudentDataBase){//从文件读入学生信息,参数为文件地址指针和StudentDataBase指针,返回值为Status类型 
	FILE* pStudentFile;
	int i=0;
	if((pStudentFile=fopen(fileName,"r"))==NULL){
		return ERROR;
	}
	while(!feof(pStudentFile)&&i<MAX_NUM_OF_STUDENTS){
		fscanf(pStudentFile,"%s\t%s%d%d%d\t%s",pStudentDataBase->studentArray[i].number,pStudentDataBase->studentArray[i].name,&pStudentDataBase->studentArray[i].chinese,&pStudentDataBase->studentArray[i].math,&pStudentDataBase->studentArray[i].english,pStudentDataBase->studentArray[i].email);
		fgetc(pStudentFile);
		i++;
	}
	fclose(pStudentFile);
	pStudentDataBase->numOfStudents=i;
	return OK;
}
Status InsertStudent(StudentDataBase* pStudentDataBase){//插入学生信息,参数为StudentDataBase指针,返回值为Status类型 
	char s[10];
	printf("输入你要插入的数据:\n");
	scanf("%s%s%d%d%d%s",pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].number,pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].name,&pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].chinese,&pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].math,&pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].english,pStudentDataBase->studentArray[pStudentDataBase->numOfStudents].email);
	pStudentDataBase->numOfStudents++;
	if(IsImformationWrong(pStudentDataBase)==YES){
		return ERROR;
	}
	return OK;
}
void SortStudentsNumber(StudentDataBase* pStudentDataBase){//按学号排序,参数为StudentDataBase指针，无返回值 
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(atoi(pStudentDataBase->studentArray[j].number	)>atoi(pStudentDataBase->studentArray[j+1].number	)){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;
}
void SortStudentsChineseDown(StudentDataBase* pStudentDataBase){//按语文降序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].chinese	<pStudentDataBase->studentArray[j+1].chinese	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;	
}
void SortStudentsMathDown(StudentDataBase* pStudentDataBase){//按数学降序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].math	<pStudentDataBase->studentArray[j+1].math	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;	
}
void SortStudentsEnglishDown(StudentDataBase* pStudentDataBase){//按英语降序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].english	<pStudentDataBase->studentArray[j+1].english	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;
}
void SortStudentsTotalDown(StudentDataBase* pStudentDataBase){//按总分降序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].english	+pStudentDataBase->studentArray[j].chinese	+pStudentDataBase->studentArray[j].math	<pStudentDataBase->studentArray[j+1].english	+pStudentDataBase->studentArray[j+1].chinese	+pStudentDataBase->studentArray[j+1].math	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;
}void SortStudentsChineseUp(StudentDataBase* pStudentDataBase){//按语文升序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].chinese	>pStudentDataBase->studentArray[j+1].chinese	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;	
}
void SortStudentsMathUp(StudentDataBase* pStudentDataBase){//按数学升序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].math	>pStudentDataBase->studentArray[j+1].math	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;	
}
void SortStudentsEnglishUp(StudentDataBase* pStudentDataBase){//按英语升序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].english	>pStudentDataBase->studentArray[j+1].english	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;
}
void SortStudentsTotalUp(StudentDataBase* pStudentDataBase){//按总分升序排序,参数为StudentDataBase指针，无返回值
	int i,j,n;
	Student t;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(pStudentDataBase->studentArray[j].english	+pStudentDataBase->studentArray[j].chinese	+pStudentDataBase->studentArray[j].math	>pStudentDataBase->studentArray[j+1].english	+pStudentDataBase->studentArray[j+1].chinese	+pStudentDataBase->studentArray[j+1].math	){
				t=pStudentDataBase->studentArray[j];
				pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				pStudentDataBase->studentArray[j+1]=t;
			}
		}
	}
	return;
}
void SortStudent(StudentDataBase* pStudentDataBase){//选择排序类型，参数为StudentDataBase指针，无返回值 
	char s[10];
	printf("输入你想要排序的方式(\nnumber\t\tchinesedown(降序)\tmathdown(降序)\nenglishdown(降序)totaldown(降序)\tchineseup(升序)\nmathup(升序)\tenglishup(升序)\t\ttotalup(升序))：\n");
	scanf("%s",s);
	if(strcmp(s,"number")==0){
		SortStudentsNumber(pStudentDataBase);
	}
	else if(strcmp(s,"chinesedown")==0){
		SortStudentsChineseDown(pStudentDataBase);
	}
	else if(strcmp(s,"mathdown")==0){
		SortStudentsMathDown(pStudentDataBase);
	}
	else if(strcmp(s,"englishdown")==0){
		SortStudentsEnglishDown(pStudentDataBase);
	}
	else if(strcmp(s,"totaldown")==0){
		SortStudentsTotalDown(pStudentDataBase);
	}
	else if(strcmp(s,"chineseup")==0){
		SortStudentsChineseUp(pStudentDataBase);
	}
	else if(strcmp(s,"mathup")==0){
		SortStudentsMathUp(pStudentDataBase);
	}
	else if(strcmp(s,"englishup")==0){
		SortStudentsEnglishUp(pStudentDataBase);
	}
	else if(strcmp(s,"totalup")==0){
		SortStudentsTotalUp(pStudentDataBase);
	}
	else{
		printf("ERROR!please input number,chinesedown,mathdown,englishdown,totaldown,chineseup,mathup,englishup,totalup\n");
		return;
	}
	return;
}
void DeleteStudentByName(StudentDataBase* pStudentDataBase){//按姓名删除学生信息，参数为StudentDataBase指针，无返回值 
	int i,j;
	char s[10];
	char name[MAX_LENGTH_OF_NAME];
	printf("请输入要删除的学生姓名:\n");
	scanf("%s",name);
	getchar();
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strcmp(pStudentDataBase->studentArray[i].name,name)==0){
			printf("该学生数据如下:\n%s\t%s\t%d\t%d\t%d\t%s\n",pStudentDataBase->studentArray[i].number,pStudentDataBase->studentArray[i].name,pStudentDataBase->studentArray[i].chinese,pStudentDataBase->studentArray[i].math,pStudentDataBase->studentArray[i].english,pStudentDataBase->studentArray[i].email);
			printf("是否删除该学生数据(Y/N):\n");
			scanf("%s",s);
			if(strcmp(s,"Y")==0){
				for(j=i;j<pStudentDataBase->numOfStudents-1;j++){
					pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				}
				pStudentDataBase->numOfStudents--;
				i--;
			}
			else{
			getchar();
			}
		}
		else if((i+1)==pStudentDataBase->numOfStudents){
			printf("there is no another student named %s\n",name);
		}
	}
	return OK;
}
void DeleteStudentByNumber(StudentDataBase* pStudentDataBase){//按学号删除学生信息，参数为StudentDataBase指针，无返回值
	int i,j;
	char s[10];
	char number[MAX_LENGTH_OF_NUMBER];
	printf("请输入要删除的学生学号:\n");
	scanf("%s",&number);
	getchar();
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strcmp(pStudentDataBase->studentArray[i].number,number)==0){
			printf("该学生数据如下:\n%s\t%s\t%d\t%d\t%d\t%s\n",pStudentDataBase->studentArray[i].number,pStudentDataBase->studentArray[i].name,pStudentDataBase->studentArray[i].chinese,pStudentDataBase->studentArray[i].math,pStudentDataBase->studentArray[i].english,pStudentDataBase->studentArray[i].email);
			printf("是否删除该学生数据(Y/N):\n");
			scanf("%s",s);
			if(strcmp(s,"Y")==0){
				for(j=i;j<pStudentDataBase->numOfStudents-1;j++){
					pStudentDataBase->studentArray[j]=pStudentDataBase->studentArray[j+1];
				}
				pStudentDataBase->numOfStudents--;
				i--;
			}
			else{
			getchar();
			}
		}
		else if((i+1)==pStudentDataBase->numOfStudents){
			printf("there is no another student numbered %s\n",number);
		}
	}
	return OK;
}
void DeleteStudent(StudentDataBase* pStudentDataBase){//选择删除方式，参数为StudentDataBase指针，无返回值 
	char s[10];
	printf("输入你想要删除的方式(number,name):\n");
	scanf("%s",s);
	if(strcmp(s,"number")==0){
		DeleteStudentByNumber(pStudentDataBase);
	}
	else if(strcmp(s,"name")==0){
		DeleteStudentByName(pStudentDataBase);
	}
	else{
		printf("ERROR!please input number or name!");
		return;
	}
	return;
}
Status PrinfFile(const char* flieName,StudentDataBase* pStudentDataBase){//将学生信息输出到文件 ，参数为文件地址和StudentDataBase指针，返回值为Status类型 
	FILE* pStudentFile;
	int i;
	if((pStudentFile=fopen(flieName,"w"))==NULL){
		return ERROR;
	}
	for(i=0;i<pStudentDataBase->numOfStudents-1;i++){
		fprintf(pStudentFile,"%s\t%s\t%d\t%d\t%d\t%s\n",pStudentDataBase->studentArray[i].number,pStudentDataBase->studentArray[i].name,pStudentDataBase->studentArray[i].chinese,pStudentDataBase->studentArray[i].math,pStudentDataBase->studentArray[i].english,pStudentDataBase->studentArray[i].email);
	}
	fprintf(pStudentFile,"%s\t%s\t%d\t%d\t%d\t%s",pStudentDataBase->studentArray[i].number,pStudentDataBase->studentArray[i].name,pStudentDataBase->studentArray[i].chinese,pStudentDataBase->studentArray[i].math,pStudentDataBase->studentArray[i].english,pStudentDataBase->studentArray[i].email);
	return OK;
}
int StatisticChinese(StudentDataBase* pStudentDataBase){//统计语文及格率和及格人数，参数为StudentDataBase指针，返回值为整型表示及格人数 
	int i,n;
	int m;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n;i++){
		if(pStudentDataBase->studentArray[i].chinese>=60&&pStudentDataBase->studentArray[i].chinese<=100){
			m++;
		}
		if(pStudentDataBase->studentArray[i].chinese<0||pStudentDataBase->studentArray[i].chinese>100){
			return -1;
		}
	}
	return m;
}
int StatisticMath(StudentDataBase* pStudentDataBase){//统计数学及格率和及格人数，参数为StudentDataBase指针，返回值为整型表示及格人数
	int i,n;
	int m;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n;i++){
		if(pStudentDataBase->studentArray[i].math>=60&&pStudentDataBase->studentArray[i].math<=100){
			m++;
		}
		if(pStudentDataBase->studentArray[i].math<0||pStudentDataBase->studentArray[i].math>100){
			return -1;
	}
	}
	return m;
}
int StatisticEnglish(StudentDataBase* pStudentDataBase){//统计英语及格率和及格人数，参数为StudentDataBase指针，返回值为整型表示及格人数
	int i,n;
	int m; 
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n;i++){
		if(pStudentDataBase->studentArray[i].english	>=60&&pStudentDataBase->studentArray[i].english	<=100){
			m++;
		}
		if(pStudentDataBase->studentArray[i].english<0||pStudentDataBase->studentArray[i].english	>100){
			return -1;
	}
	}
	return m;
}
int StatisticTotal(StudentDataBase* pStudentDataBase){//统计三科都及格率和及格人数，参数为StudentDataBase指针，返回值为整型表示及格人数
	int i,n;
	int m;
	n=pStudentDataBase->numOfStudents;
	for(i=0;i<n;i++){
		if(pStudentDataBase->studentArray[i].english	>=60&&pStudentDataBase->studentArray[i].english	<=100&&pStudentDataBase->studentArray[i].chinese	>=60&&pStudentDataBase->studentArray[i].chinese	<=100&&pStudentDataBase->studentArray[i].math	>=60&&pStudentDataBase->studentArray[i].math	<=100){
			m++;
		}
		if(pStudentDataBase->studentArray[i].english<0||pStudentDataBase->studentArray[i].english	>100||pStudentDataBase->studentArray[i].chinese<0||pStudentDataBase->studentArray[i].chinese>100||pStudentDataBase->studentArray[i].math<0||pStudentDataBase->studentArray[i].math>100){
			return -1;
		}
	}
	return m;
	}

void Statistic(StudentDataBase* pStudentDataBase){//选择统计方式，参数为StudentDataBase指针，无返回值 
	char s[10];
	int a,m=0;
	printf("请输入你想要统计的方式（\nchinese\nmath\nenglish\ntotal）\n(如结果为负，则成绩有错误)：\n");
	scanf("%s",s);
	if(strcmp(s,"chinese")==0){
		a=StatisticChinese(pStudentDataBase);
		printf("语文及格率为%f%%,及格人数为%d,不及格人数为%d\n",100.0*a/pStudentDataBase->numOfStudents,a,pStudentDataBase->numOfStudents-a);
	}
	else if(strcmp(s,"math")==0){
		a=StatisticMath(pStudentDataBase);
		printf("数学及格率为%f%%,及格人数为%d,不及格人数为%d\n",100.0*a/pStudentDataBase->numOfStudents,a,pStudentDataBase->numOfStudents-a);
	}
	else if(strcmp(s,"english")==0){
		a=StatisticEnglish(pStudentDataBase);
		printf("英语及格率为%f%%,及格人数为%d,不及格人数为%d\n",100.0*a/pStudentDataBase->numOfStudents,a,pStudentDataBase->numOfStudents-a);
	}
	else if(strcmp(s,"total")==0){
		a=StatisticTotal(pStudentDataBase);
		printf("三科均及格率为%f%%,及格人数为%d,不及格人数为%d\n",100.0*a/pStudentDataBase->numOfStudents,a,pStudentDataBase->numOfStudents-a);
	}
	else{
		printf("please input chinese,math,english,total!");
	}
	return;
}
float ChineseAverage(StudentDataBase* pStudentDataBase,float a){
	int i;
	float s=0;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		s+=pStudentDataBase->studentArray[i].chinese;
	}
	a=s/pStudentDataBase->numOfStudents;
	return a;
}
float MathAverage(StudentDataBase* pStudentDataBase,float a){
	int i;
	float s=0;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		s+=pStudentDataBase->studentArray[i].math;
	}
	a=s/pStudentDataBase->numOfStudents;
	return a;
}
float EnglishAverage(StudentDataBase* pStudentDataBase,float a){
	int i;
	float s=0;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		s+=pStudentDataBase->studentArray[i].english;
	}
	a=s/pStudentDataBase->numOfStudents;
	return a;
}
float TotalAverage(StudentDataBase* pStudentDataBase,float a){
	int i;
	float s=0;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		s+=pStudentDataBase->studentArray[i].english+pStudentDataBase->studentArray[i].math+pStudentDataBase->studentArray[i].english;
	}
	a=s/pStudentDataBase->numOfStudents;
	return a;
}
void Average(StudentDataBase* pStudentDataBase){
	char s[10];
	float a,b;
	printf("请输入你想要求平均成绩的方式(chinese,math,english,total):\n");
	scanf("%s",s);
	if(strcmp(s,"chinese")==0){
		a=ChineseAverage(pStudentDataBase,b);
		printf("语文平均成绩为%f",a);
	}
	else if(strcmp(s,"math")==0){
		a=MathAverage(pStudentDataBase,b);
		printf("数学平均成绩为%f",a);
	}
	else if(strcmp(s,"english")==0){
		a=EnglishAverage(pStudentDataBase,b);
		printf("英语平均成绩为%f",a);
	}
	else if(strcmp(s,"total")==0){
		a=TotalAverage(pStudentDataBase,b);
		printf("三科平均成绩为%f",a);
	}
	else{
		printf("please input chinese,math,english,total!");
	}
}
void OperationSelect(const char* fileName,Student* pStudent,StudentDataBase* pStudentDataBase){//选择操作类型，参数为文件地址，Student指针和StudentDataBase指针，无返回值 
	char s[16];
	printf("请输入你想要进行的操作(\nseek(搜索学生信息)\ninsert(插入学生信息)\ndelete(删除学生信息)\nsort(成绩排序)\nstatistic(及格率统计)\naverage(求平均成绩)\nonestudent(对某个学生信息进行操作)):\n");
	scanf("%s",s);
	if(strcmp(s,"seek")==0){
		SeekStudent(pStudentDataBase);
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else if(strcmp(s,"insert")==0){
		InsertStudent(pStudentDataBase);
		PrintAllStudents(pStudentDataBase);
		printf("是否输出到文件(Y是/N否):\n");
		scanf("%s",s);
		if(strcmp(s,"Y")==0){
			PrinfFile(fileName,pStudentDataBase);
		}
		else{
			PrintAllStudents(pStudentDataBase);	
		}
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else if(strcmp(s,"delete")==0){
		DeleteStudent(pStudentDataBase);
		PrintAllStudents(pStudentDataBase);
		printf("是否输出到文件（Y是/N否）：\n");
		scanf("%s",s);
		if(strcmp(s,"Y")==0){
			PrinfFile(fileName,pStudentDataBase);
		}
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
		scanf("%s",s);
		if(strcmp(s,"Y")==0){
			OperationSelect(fileName,pStudent,pStudentDataBase);
		}
		else{
			printf("Thanks for using!\n");
		}
	}
	else if(strcmp(s,"sort")==0){
		SortStudent(pStudentDataBase);
		PrintAllStudents(pStudentDataBase);
		printf("是否输出到文件(Y是/N否):\n");
		scanf("%s",s);
		if(strcmp(s,"Y")==0){
			PrinfFile(fileName,pStudentDataBase);
		}
		else{
			PrintAllStudents(pStudentDataBase);	
		}
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else if(strcmp(s,"statistic")==0){
		Statistic(pStudentDataBase);
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else if(strcmp(s,"average")==0){
		Average(pStudentDataBase);
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else if(strcmp(s,"onestudent")==0){
		OneStudent(pStudentDataBase);
		PrintAllStudents(pStudentDataBase);
		printf("是否输出到文件(Y是/N否):\n");
		scanf("%s",s);
		if(strcmp(s,"Y")==0){
			PrinfFile(fileName,pStudentDataBase);
		}
		else{
			PrintAllStudents(pStudentDataBase);	
		}
		printf("是否需要进行其它操作（Y(是)/N(否)）:\n");
	scanf("%s",s);
	if(strcmp(s,"Y")==0){
		OperationSelect(fileName,pStudent,pStudentDataBase);
	}
	else{
		printf("Thanks for using!\n");
	}
	}
	else{
		printf("please input seek,insert,delete,sort,statistic,average,onestudent!");
	}
	return;
}
void OneStudent(StudentDataBase* pStudentDataBase){//对单个学生信息进行操作，参数为StudentDataBase指针，无返回值 
	char s[100];
	int i;
	printf("请输入你想要操作的学生的姓名或学号:\n");
	scanf("%s",s);
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strcmp(s,pStudentDataBase->studentArray[i].number)==0||strcmp(s,pStudentDataBase->studentArray[i].name)==0){
			PrintStudent(&pStudentDataBase->studentArray[i]);
			break;
		}
		else if(i+1==pStudentDataBase->numOfStudents	){
			printf("there isn't this student!");
		}
	}
	printf("请输入你想要进行的操作(revise(修改),delete(删除)):\n");
	scanf("%s",s);
	if(strcmp(s,"revise")==0){
		ReviseStudent(&pStudentDataBase->studentArray[i]);
	}
	else if(strcmp(s,"delete")==0){
		DeleteOneStudent(pStudentDataBase,i);
	}
	else{
		printf("please input revise,delete!");
	}
	return;
}
Status ReviseStudent(Student* pStudent){//选择替换方式，参数为Student指针，返回值为Status类型（ERROR表示错误，OK表示成功）
	char s[100];
	int a;
	printf("请输入你想要修改的方式(\nchinese(语文)\nmath(数学)\nenglish(英语)\nall(全部)):\n");
	scanf("%s",s);
	if(strcmp(s,"chinese")==0){
		printf("请输入新的语文成绩:\n");
		ReviseStudentChinese(pStudent);
	}
	else if(strcmp(s,"math")==0){
		printf("请输入新的数学成绩:\n");
		
	}
	else if(strcmp(s,"english")==0){
		printf("请输入新的英语成绩:\n");
		ReviseStudentEnglish(pStudent);
	}
	else if(strcmp(s,"all")==0){
		printf("请输入新的学生信息:\n");
		ReviseStudentAll(pStudent);
	}
	else{
		printf("please input chinese,math,english,all!");
		return ERROR;
	}
	return OK;
}
void ReviseStudentChinese(Student* pStudent){//替换学生语文成绩，参数为Student指针，无返回值 
	int a;
	scanf("%d",&a);
	pStudent->chinese=a;
	return;
} 
void ReviseStudentMath(Student* pStudent){//替换学生数学成绩，参数为Student指针，无返回值 
	int a;
	scanf("%d",&a);
	pStudent->math=a;
	return;
} 
void ReviseStudentEnglish(Student* pStudent){//替换学生英语成绩，参数为Student指针，无返回值 
	int a;
	scanf("%d",&a);
	pStudent->english=a;
	return;
}
void ReviseStudentAll(Student* pStudent){//替换学生整体信息，参数为Student指针，无返回值 
	scanf("%s%s%d%d%d%s",pStudent->number,pStudent->name,&pStudent->chinese,&pStudent->math,&pStudent->english,pStudent->email);
	return;
} 
Status DeleteOneStudent(StudentDataBase* pStudentDataBase,int a){//删除单个学生信息，参数为StudentDataBase指针和整型变量（表示该学生所在序号），返回值为Status类型（ERROR表示错误，OK表示成功） 
	int i;
	i=a;
	for(i;i<pStudentDataBase->numOfStudents-1	;i++){
		pStudentDataBase->studentArray[i]=pStudentDataBase->studentArray[i+1];
	}
	pStudentDataBase->numOfStudents--;
	return OK;
}
void SeekStudent(StudentDataBase* pStudentDataBase){//查找学生信息，参数为StudentDataBase指针，无返回值 
	char s[100];
	int i;
	printf("请输入你想要搜索的学生的姓名或学号:\n");
	scanf("%s",s);
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strcmp(s,pStudentDataBase->studentArray[i].number)==0||strcmp(s,pStudentDataBase->studentArray[i].name)==0){
			PrintStudent(&pStudentDataBase->studentArray[i]);
			break;
		}
		else if(i+1==pStudentDataBase->numOfStudents	){
			printf("there isn't this student!\n");
		}
	}
	return;
}
Bool IsNumberWrong(StudentDataBase* pStudentDataBase){//判断学号是否错误，参数为StudentDataBase指针，返回值为Bool类型（YES为有错误，NO没有错误） 
	int i;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strlen(pStudentDataBase->studentArray[i].number)!=13){
			return YES;
		}
	}
	return NO;
}
Bool IsScoreWrong(StudentDataBase* pStudentDataBase){//判断分数是否错误，参数为StudentDataBase指针，返回值为Bool类型（YES为有错误，NO没有错误） 
	int i;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(pStudentDataBase->studentArray[i].english<0||pStudentDataBase->studentArray[i].english	>100||pStudentDataBase->studentArray[i].chinese<0||pStudentDataBase->studentArray[i].chinese>100||pStudentDataBase->studentArray[i].math<0||pStudentDataBase->studentArray[i].math>100){
			return YES;
		}
	}
	return NO;
}
Bool IsEmailWrong(StudentDataBase* pStudentDataBase){//判断邮箱是否错误，参数为StudentDataBase指针，返回值为Bool类型（YES为有错误，NO没有错误） 
	char a[2]="@",b[5]=".com";
	int i;
	for(i=0;i<pStudentDataBase->numOfStudents;i++){
		if(strstr(pStudentDataBase->studentArray[i].email,a)==NULL||strstr(pStudentDataBase->studentArray[i].email,b)==NULL){
			return YES;	
		}
	}
	return NO;
}
Bool IsImformationWrong(StudentDataBase* pStudentDataBase){//判断学生信息是否错误，参数为StudentDataBase指针，返回值为Bool类型（YES为有错误，NO没有错误） 
	if(IsNumberWrong(pStudentDataBase)==YES){
		printf("the numbers are wrong!\n");
		return YES;
	}
	if(IsScoreWrong(pStudentDataBase)==YES){
		printf("the scores are wrong!\n");
		return YES;
	}
	if(IsEmailWrong(pStudentDataBase)==YES){
		printf("the emails are wrong!\n");
		return YES;
	}
	return NO;
}

int main(){
	StudentDataBase studentDataBase;
	Student student;
	if(ReadStudentFile("F:\\文档\\student.txt",&studentDataBase)==NULL){
		printf("Failed to read the file\n");
		return 1;
	}
	if(IsImformationWrong(&studentDataBase)==NO){
		printf("学号		姓名	语文	数学	英语	邮箱\n");
		PrintAllStudents(&studentDataBase);
		OperationSelect("F:\\文档\\student.txt",&student,&studentDataBase);
	}
	else{
		PrintAllStudents(&studentDataBase);
	}
	return 0;
}
