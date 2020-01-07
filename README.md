# sheepkeeper

## 백준 3187번 양치기 꿍 풀이입니다.

### 먼저 동적 2차원 배열을 선언합니다.
```c++
int r; //가로길이
    int c; //세로길이
    
    cin >> r >> c;
    //동적 2차원 배열 선언
    char** world = new char*[r];
    bool** visit = new bool*[r];
    for(int count = 0 ; count < r; ++count){
        world[count] = new char[c];
        visit[count] = new bool[c];
    }
```
### 마을을 만들어 줍니다. (문자를 입력 받습니다)
```c++
for(int i=0; i<r; i++){
        for(int j=0; j<c; j++){
            cin >> world[i][j];
            visit[i][j] = false;
        }
    }
```
배열로 마을을 구성했습니다.

### BFS(너비우선탐색)의 원리를 이용해서 탐색합니다
```c++
for(int i=0; i<r; i++){
        for(int j=0; j<c; j++){
            if(world[i][j] != '#' && visit[i][j] == false){
                
                int v=0;
                int k=0;
                
                queue<int> a;
                queue<int> b;
                
                a.push(i);
                b.push(j);
                visit[i][j] = true;
                while(!a.empty()){
                    int x = a.front();
                    int y = b.front();
                    a.pop();
                    b.pop();
                    
                    if(world[x][y] == 'v')
                        v++;
                    if(world[x][y] == 'k')
                        k++;
                    //동쪽
                    if(y<c-1){
                        if(world[x][y+1] != '#' && visit[x][y+1] == false){
                            a.push(x);
                            b.push(y+1);
                            visit[x][y+1] = true;
                        }
                    }
                    //남쪽
                    if(x<r-1){
                        if(world[x+1][y] != '#' && visit[x+1][y] == false){
                            a.push(x+1);
                            b.push(y);
                            visit[x+1][y] = true;
                        }
                    }
                    //서쪽
                    if(y>0){
                        if(world[x][y-1] != '#' && visit[x][y-1]
                           ==false){
                            a.push(x);
                            b.push(y-1);
                            visit[x][y-1] = true;
                        }
                    }
                    //북쪽
                    if(x>0){
                        if(world[x-1][y] != '#' && visit[x-1][y]
                           ==false){
                            a.push(x-1);
                            b.push(y);
                            visit[x-1][y] = true;
                        }
                    }
                    
                    
                }
                //어느쪽이 살아남을 까
                if(v<k)
                    sheep += k;
                else
                    wolf += v;
                
            }
            
        }
    }
```
배열로 구성된 마을에서 1) #문자가 아닌곳 2) 방문하지 않은 곳을 차례로 선택해서 BFS를 진행합니다.\
선택된 정점의 행, 열 값을 queue에 각각 넣고 BFS의 원리를 적용합니다.\
각 정점마다 방문한 정점이 v인지 k인지 조사를 하고, 동서남북위치의 정점을 조사해서 방문하지 않은 곳이라면 queue에 넣고 방문처리를 해줍니다.\
BFS탐색을 하는 곳은 #(울타리)로 둘러쌓여있는 곳이므로 그때마다 울타리 안에 있는 양과 늑대의 수를 비교해 전역번수로 선언된 양과 늑대의 수에 추가를 해줍니다.\
이 방법을 모든정점에 대해, 그리고 방문하지 않은 정점, 울타리가 아닌 정점에 대해 진행을 하고 양과 늑대의 수를  프로그램이 완성됩니다.\

## 전체 코드는 다음 입니다.

```c++
#include <iostream>
#include <queue>

using namespace std;

int wolf=0;//살아남는 늑대 수
int sheep=0;//살아남는 양 수


int main() {
    int r; //가로길이
    int c; //세로길이
    
    cin >> r >> c;
    //동적 2차원 배열 선언
    char** world = new char*[r];
    bool** visit = new bool*[r];
    for(int count = 0 ; count < r; ++count){
        world[count] = new char[c];
        visit[count] = new bool[c];
    }
    
    //마을 만들기
    for(int i=0; i<r; i++){
        for(int j=0; j<c; j++){
            cin >> world[i][j];
            visit[i][j] = false;
        }
    }
    //bfs로 탐색하자
    //조건 : 한번도 탐색하지 않은 곳만 탐색하기
    //나는 '#'이 아닌 곳중에서 visit=false 인 곳만 탐색했음.
    
    for(int i=0; i<r; i++){
        for(int j=0; j<c; j++){
            if(world[i][j] != '#' && visit[i][j] == false){
                /*
                배열로 bfs 구현하기
                연결된 엣지는 (다음행, 다음열)(but #이 아니고 visit이 false)
                각 정점 방문 할 때마다 v인지 k인지 체크하고 한개씩 올려줌
                 
                c로 엣지로연결됏다고생각하는곳으로 어캐이동하징
                 front의 i,j를 가져와? ㅇㅇ
                 
                
                */
                int v=0;
                int k=0;
                
                queue<int> a;
                queue<int> b;
                
                a.push(i);
                b.push(j);
                visit[i][j] = true;
                while(!a.empty()){
                    int x = a.front();
                    int y = b.front();
                    a.pop();
                    b.pop();
                    
                    if(world[x][y] == 'v')
                        v++;
                    if(world[x][y] == 'k')
                        k++;
                    //동쪽
                    if(y<c-1){
                        if(world[x][y+1] != '#' && visit[x][y+1] == false){
                            a.push(x);
                            b.push(y+1);
                            visit[x][y+1] = true;
                        }
                    }
                    //남쪽
                    if(x<r-1){
                        if(world[x+1][y] != '#' && visit[x+1][y] == false){
                            a.push(x+1);
                            b.push(y);
                            visit[x+1][y] = true;
                        }
                    }
                    //서쪽
                    if(y>0){
                        if(world[x][y-1] != '#' && visit[x][y-1]
                           ==false){
                            a.push(x);
                            b.push(y-1);
                            visit[x][y-1] = true;
                        }
                    }
                    //북쪽
                    if(x>0){
                        if(world[x-1][y] != '#' && visit[x-1][y]
                           ==false){
                            a.push(x-1);
                            b.push(y);
                            visit[x-1][y] = true;
                        }
                    }
                    
                    
                }
                //어느쪽이 살아남을 까
                if(v<k)
                    sheep += k;
                else
                    wolf += v;
                
            }
            
        }
    }
    
    //살아남게 되는 양과 늑대의 수 순서대로 출력
    cout << sheep << "\t" << wolf <<endl;
    
    /*
    //동적 2차원 배열 해제
    for(int count=0; count <10; ++count){
        delete[] world[count];
        delete[] visit[count];
    }
    delete[] world;
    delete[] visit;
     */
}
```

