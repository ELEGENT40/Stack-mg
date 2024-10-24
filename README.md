# Stack-mg
栈解决迷宫问题
#include <iostream>
#include <malloc.h>
using namespace std;
#define Maxsize 1000
int mg[8 + 2][8 + 2] = { {1,1,1,1,1,1,1,1,1,1} ,{1,0,0,1,0,0,0,1,0,1} ,
							{1,0,0,1,0,0,0,1,0,1} ,{1,0,0,0,0,1,1,0,0,1} ,
							{1,0,1,1,1,0,0,0,0,1} ,{1,0,0,0,1,0,0,0,0,1} ,
							 {1,0,1,0,0,0,1,0,0,1} ,{1,0,1,1,1,0,1,1,0,1} ,
							 {1,1,0,0,0,0,0,0,0,1} ,{1,1,1,1,1,1,1,1,1,1} };
//int mg[4 + 2][4 + 2] = { {1,1,1,1,1,1},{1,0,0,0,1,1},{1,1,0,1,1,1},{1,0,0,0,0,1},{1,1,0,1,0,1},{1,1,1,1,1,1} };

typedef struct {
	int x, y, di;
}Box;
typedef struct {
	Box data[Maxsize];
	int top;
}Sqstack;


Sqstack gst = { {0},-1 };

void InitStack(Sqstack& s) {
	s.top = -1;
}

void Push(Sqstack& s, Box e) {//进栈
	if (s.top == Maxsize - 1) {
		cout << "error" << endl;
	}
	else {
		s.top++;
		s.data[s.top] = e;
	}
}
void Pop(Sqstack& s, Box& e) {
	if (s.top == -1) {
		cout << "error" << endl;
	}
	else {
		e = s.data[s.top];
		s.top--;
	}
}
void GetTop(Sqstack& s, Box& e) {
	if (s.top == -1) {
		cout << "error" << endl;
	}
	else {
		e = s.data[s.top];
	}
}
bool StackEmpty(const Sqstack& s) {
    return s.top == -1;
}

bool mypath(int xi, int yi, int xe, int ye) {
	Box path[Maxsize],e;
	int i, j, di, il, jl, k;
	bool find;
	Sqstack s;
	InitStack(s);
	e.x = xi, e.y = yi, e.di = -1;
	Push(s, e);
	mg[e.x][e.y] = -1;

	while (!StackEmpty(s)){
		GetTop(s, e);
		i = e.x, j = e.y, di = e.di;


		if (i == xe && j == ye) {
			cout << "路径如下：" << endl;
			k = 0;
			while (!StackEmpty(s)) {
				Pop(s, e);
				path[k++] = e;
			}
			while (k > 0) {
				cout << "(" << path[k - 1].x << "," << path[k - 1].y << ")" ;
				if ((k + 1) % 5 == 0) {
					cout << endl;
				}
				k--;
			}
			return true;
		}
		find = false;
		while (di < 4 && !find) {
			
			di++;
			switch (di) {
			case 0:il = i - 1, jl = j; break;
			case 1:il = i , jl = j + 1; break;
			case 2:il = i + 1, jl = j; break;
			case 3:il = i , jl = j - 1; break;
			}
			if (mg[il][jl] == 0) {
				find = true;
				
			}
		}
		if (find) {
			s.data[s.top].di = di;//将下一个可走方块进栈
			e.x = il, e.y = jl, e.di = -1;
		
			Push(s, e);
			mg[i ][j] = -1;
		}
		else {//回溯
			
			Pop(s, e);
			mg[i][j ] = -1;
			GetTop(s, e);
		
			mg[e.x][e.y] = 0;
		}
	}
	
	return false;
};




int main() {
	
	if (!mypath(1, 1, 8, 8))
		cout << "无解!cnm" << endl;

	return 0;
}
