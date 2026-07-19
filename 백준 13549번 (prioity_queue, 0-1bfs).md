안녕하세요
박장훈 입니다


​

저번에 이어서 백준 문제 풀이 시작하겠습니다
분명히 벨만포드 복습먼저 한다고 했었는데 ㅋㅋㅋ 갑자기 복습하고 싶은 문제가 문득 떠올라서 그 문제부터 복습을 해버렸습니다

​
그래서 순서를 변경해서 숨바꼭질3 문제를 먼저 풀어보도록 하겠습니다
​
<img width="900" height="941" alt="image" src="https://github.com/user-attachments/assets/32232b90-b3c3-4d5e-973c-f3b7958b6004" />

​이 문제는 시작이 되는 위치에서 목표가 되는 위치까지 이동을 하는데, 
### 이때 최소의 시간을 이용해서 목표지점에 도달해야하는게 목표입니다

​
그리고 그 최소 값을 출력하는게 목표입니다
이때 움직일수 있는 경우의 수는 걸리는 시간이 0인 현재 위치의2배인 곳으로 이동, 
걸리는 시간이 1인 +1,-1만큼 걸어서 이동하기  이 두개가 있습니다

이렇게 총 3개를 이용하여 목표지점까지 이동을 해야합니다

## 아이디어
일단 가중치가 0인 현재 위치의 2배로 순간이동하는 방법과 가중치가 1로 +1, -1 방법이니 bfs를 이용하여 최단거리 구하기는 불가능하고, 
dp같은 경우에는 규칙적인 점화식이 있어야하는데 방향이 두가지 방향으로 이동하고 가중치가 있으므로 적절하지 않다고 생각했습니다.

### 따라서 정석적인 priority_queue를 이용하는 방법과  가중치가 0과1 두개 밖에 없으니 0-1BFS 를 이용하는 총 2가지 방법을 이용할 수 있을것 같습니다






## 첫번째

​구조를 만들때 0-1 bfs인 deque를 이용해서 만들어 보도록 하겠습니

​처음 시작해주는 위치를   push_back을 이용하여 만들어 줍니다

이때, q.first()는 시작하는 위치, 즉 N이고 q.second()는 걸리는 시간입니다. 
또한 visited는 중복으로 같은 위치에 들어가는 것을 막기 위해서 만들었습니다.

// 처음 초기화

<img width="322" height="162" alt="image" src="https://github.com/user-attachments/assets/ef780b56-a337-4e70-88af-33294a67e4c7" />


이후에 q.front와 auto를 이용해서 q.first()를 pos로 q.second()를 time으로 변수를 만들어줍니다

// 변수 생성

<img width="447" height="217" alt="image" src="https://github.com/user-attachments/assets/83ee34e2-40fa-43f9-a6a8-9f613e615a50" />

이후에 메인이 되는 지점인 pos가 도착지점인 K에 도착했을때와 pos*2 , pos-1, pos+1 일때로 나눠줍니다

​

​

pos*2일때는 최대의수 100000을 넘지말아야하며 중복으로 도착하는 막기 위해서 visited가 false일때만 도착하게 만들어줍니다.
또한, 가중치가 0이기 때문에 맨 앞에 있어야 합니다
따라서, q.push_front(); 로 넣어주어야합니다

// 2*x

<img width="763" height="225" alt="image" src="https://github.com/user-attachments/assets/8a51d29f-e53a-4721-82c7-fd777743e921" />



나머지 두개도 마찬가지로 pos는 0보다 크고, 100000보다 작아야 하며, 우선순위가 pos*2 보다 낮기 때문에 q.push_back();으로 넣어줘야 합니다

//X+1, X-1

<img width="827" height="454" alt="image" src="https://github.com/user-attachments/assets/b6493dd2-a737-4adc-addc-b7d42a0f6e22" />

이후에 이전에 도착지점에서 만들었던,  result를 출력해줍니다

<img width="374" height="112" alt="image" src="https://github.com/user-attachments/assets/c939da78-dca3-4fd9-a6a8-f79608127f72" />





이렇게 하면 0-1 bfs를 이용한 코드는 완성이 됩니다

//코드

```cpp
#include<iostream>

#include<deque>

using namespace std;

​

int N,K,result=0;

deque<pair<int, int>> q;

bool visited[100001];

​

​

int main(){

​

cin>>N>>K;

q.push_back({N,0});

visited[N] =true;

​

while(!q.empty()){

​

auto[pos, time]= q.front();

q.pop_front();

​

if(pos==K){

result = time;

break;

}

​

//2*x 위치로

if( pos*2 <=100000 && visited [pos*2]==false){

visited [pos*2]=true;

q.push_front({pos*2, time});

}

​

//X-1 위치로

if( pos-1 >= 0 && visited [pos-1]==false){

visited [pos-1]=true;

q.push_back({pos-1, time+1});

}

​

//X+1 위치로

if( pos+1 <= 100000 && visited [pos+1]==false){

visited [pos+1]=true;

q.push_back({pos+1, time+1});

}

​

}

​

cout << result << '\n';

return 0;

}
```


## 두 번째

다익스트라 알고리즘을 이용한 코드
변경사항이 약간 있을뿐, 전반적인 코드는 동일합니다

​

먼저 q를 만들때, deque가 아니라
priority_queue<pair<int, int>, vector<pair<int,int>>, greater<pair<int, int>> >q;
으로 q를 만들어줍니다

​
이때, 항상 다익스트라는 앞에있는 숫자 즉 q.first()를 기준으로 가중치를 설정하기 때문에, 여기서는, T가 first, 현위치가 second로 가야합니다
따라서 처음 q에 넣을때도 q.push({0,N});으로 넣어야합니다

//q. push({0,N});코드

<img width="510" height="240" alt="image" src="https://github.com/user-attachments/assets/79de56cc-2e41-420d-9931-e819906f11ba" />


이후에 아래 코드는 거의 동일합니다.
다만, 0-1bfs와 다른점은 push_back이나 push_front를 사용하지 않고, push를 이용해 줍니다.

//위치 이동하는 코드

<img width="808" height="697" alt="image" src="https://github.com/user-attachments/assets/562a8071-ab86-4eb3-8b57-e01509831815" />







이렇게 하면, 나머지는 0-1 bfs와 동일하게 하면됩니다

//코드

```cpp
#include<iostream>
#include<queue>
#include<vector>

using namespace std;

​
int N,K,result=0;
priority_queue<pair<int, int>, vector<pair<int,int>>, greater<pair<int, int>> >q;
bool visited[100001];

​
int main(){
// q에 들어갈거 (가중치 즉 T , 현위치)
cin>>N>>K;
q.push({0,N});
visited[N] =true;


while(!q.empty()){
auto[time, pos]= q.top();
q.pop();

if(pos==K){
result = time;
break;
}

//2*x 위치로
if( pos*2 <=100000 && visited [pos*2]==false){
visited [pos*2]=true;
q.push({time, pos*2});
}

//X-1 위치로
if( pos-1 >= 0 && visited [pos-1]==false){
visited [pos-1]=true;
q.push({time+1, pos-1});
}

//X+1 위치로
if( pos+1 <= 100000 && visited [pos+1]==false){
visited [pos+1]=true;
q.push({time+1, pos+1});
  }
}

cout << result << '\n';
return 0;
}
```


## 실수한 점

### * 0-1 bfs에서 실수한점
pos의 위치를 변경할때  if(pos *2 <=100000 && visited[pos*2]==false); 
해줘야 합니다  만약 if(visited[pos*2] ==false && pos * 2 <= 100000);
와 같이 해준다면, visited[pos*2]가 인식이 돼서 범위(out of bound)를 벗어나는 오류가 일어날 수 도 있습니다

​

visited를 다룰때 목표지점에서 if(!visited[pos]) 를 만들어서는 안됩니다
이전에 그 위치에 도달을 해서 visitd[pos]를 미리 true로 만들어 놨었기 때문에 목표지점에서 무조건 적으로 false로 고정이 되기 때문입니다

### * 다익스트라 알고리즘에서 틀린 점
priority_queue는 #include<queue> 헤더파일을 이용하면 사용이 가능하며,
queue와 priority_queue 모두 값을 넣을 때 q.push({1,2}); 이런식으로 넣어 주어야 합니다. 하지만 deque와 vector는 push_back({1,2}); 이런식으로 넣어주어야 하니,  헷갈리지 말아야 합니다.

​

priority_queue에서 맨앞을 나타내는 함수는 q.top(); 이고 
deque와 queue에서는 front(); 입니다.




