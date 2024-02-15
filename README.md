# IUR
C++AI井字棋
'''
#include<bits/stdc++.h>
#include<windows.h>
#include<conio.h>
#define endl "\n"
using namespace std;
short mp[4][4]={};
void COLOR(int SB_0){
	switch(SB_0%15){
		case 0: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_RED|FOREGROUND_GREEN|FOREGROUND_BLUE);return;
		case 1: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_GREEN|FOREGROUND_BLUE);return;
		case 2: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_GREEN);return;
		case 3: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_RED|FOREGROUND_BLUE);return;
		case 4: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_RED);return;
		case 5: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_RED|FOREGROUND_GREEN);return;
		case 6: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY|FOREGROUND_BLUE);return;
		case 7: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_RED|FOREGROUND_GREEN|FOREGROUND_BLUE);return;
		case 8: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_RED);return;
		case 9: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),BACKGROUND_INTENSITY|BACKGROUND_GREEN|BACKGROUND_BLUE);return;
		case 10: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),BACKGROUND_INTENSITY|BACKGROUND_RED|BACKGROUND_BLUE);return;
		case 11: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_RED|FOREGROUND_BLUE);return;
		case 12: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_RED|FOREGROUND_GREEN);return;
		case 13: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_INTENSITY);return;
		case 14: SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_GREEN|FOREGROUND_BLUE);return;
	}
	return ;
}
void c(){
	system("cls");
	for(int i=1;i<=3;i++){
		for(int j=1;j<=3;j++)
			if(mp[i][j]==0)cout<<'+';
			else if(mp[i][j]==1)cout<<'*';
			else cout<<'#';
		cout<<endl;
	}
}
bool pd(int ls){
	for(int i=1;i<=3;i++){
		int j=1;
		for(j;mp[i][j]==ls;j++);
		if(j==4)return true;
	}
	for(int i=1;i<=3;i++){
		int j=1;
		for(j;mp[j][i]==ls;j++);
		if(j==4)return true;
	}
	if((mp[1][1]==ls&&mp[2][2]==ls&&mp[3][3]==ls)||(mp[1][3]==ls&&mp[2][2]==ls&&mp[3][1]==ls))
		return true;
	return false;
}
struct ok{
	int x=0,y=0,rjz=-1,ajz=-1;
};
bool cmp(ok a,ok b){
	if((a.ajz+a.rjz)!=(b.ajz+b.rjz))return (a.ajz+a.rjz)>(b.ajz+b.rjz);
	if(a.ajz!=b.ajz)return a.ajz>b.ajz;
	return rand()%2;
}
int main(){
	COLOR(0);
	srand((unsigned)time(NULL));
	short cs=1;
	int x=2,y=2;
	while(cs++){
		char ls;
		while(1){
			system("cls");
			for(int i=1;i<=3;i++){
				for(int j=1;j<=3;j++)
					if(i==x&&j==y){
						COLOR(1);
						if(mp[i][j]==0)cout<<'+';
						else if(mp[i][j]==1)cout<<'*';
						else cout<<'#';
						COLOR(0);
					}else if(mp[i][j]==0)cout<<'+';
					else if(mp[i][j]==1)cout<<'*';
					else cout<<'#';
				cout<<endl;
			}
			ls=_getch();
			if(ls=='w')x--;
			if(ls=='s')x++;
			if(ls=='a')y--;
			if(ls=='d')y++;
			x=max(1,min(3,x));
			y=max(1,min(3,y));
			if(ls==' '&&mp[x][y]==0)break;
		}
		c();
		if(mp[x][y]!=0)continue;
		mp[x][y]=1;
		if(pd(1)){c();cout<<"User 1 * flag!!!";return 0;}
		if(cs==6){c();cout<<"OH,All flag";return 0;} 
		if(cs==2&&((x==2&&y==2)||abs(x-y)==1))mp[(rand()%2)*2+1][(rand()%2)*2+1]=-1;
		else{
			for(int i=1;i<=3;i++)
				for(int j=1;j<=3;j++)
					if(mp[i][j]==0){
						mp[i][j]=-1;
						if(pd(-1)){c();cout<<"AI -1.# flag!!!";return 0;} 
						mp[i][j]=0;
					}
			bool iu=0;
			for(int i=1;i<=3&&!iu;i++)
				for(int j=1;j<=3;j++)
					if(mp[i][j]==0){
						mp[i][j]=1;
						if(pd(1)){mp[i][j]=-1;iu=1;break;} 
						mp[i][j]=0;
					}
			if(!iu){
				ok sb[10]={};
				for(int i=1;i<=3;i++)
					for(int j=1;j<=3;j++){
						if(mp[i][j]!=0)continue;
						sb[(i-1)*3+j].x=i;sb[(i-1)*3+j].y=j;
						sb[(i-1)*3+j].ajz=sb[(i-1)*3+j].rjz=0;
						for(int k=1;k<=3;k++){
							sb[(i-1)*3+j].ajz+=((mp[i][k]==-1)+(mp[k][j]==-1));
							sb[(i-1)*3+j].rjz+=((mp[i][k]==1)+(mp[k][j]==1));
						}
						if(i==j){
							sb[(i-1)*3+j].ajz+=((mp[1][1]==-1)+(mp[2][2]==-1)+(mp[3][3]==-1));
							sb[(i-1)*3+j].rjz+=((mp[1][1]==1)+(mp[2][2]==1)+(mp[3][3]==1));
						}
						if(i+j==4){
							sb[(i-1)*3+j].ajz+=((mp[1][3]==-1)+(mp[2][2]==-1)+(mp[3][1]==-1));
							sb[(i-1)*3+j].rjz+=((mp[1][3]==1)+(mp[2][2]==1)+(mp[3][1]==1));
						}
						sort(sb+1,sb+9,cmp);
					}
				mp[sb[1].x][sb[1].y]=-1;
			}
			if(pd(-1)){c();cout<<"AI -1.# flag!!!";return 0;} 
		}
	}
	return 0;
}
'''
