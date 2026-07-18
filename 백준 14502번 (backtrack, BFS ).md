저번에 이어서 bfs를 이용한 문제를 풀어보려고 합니다


지난번에 풀었던 bfs문제는 visited를 3차원 배열로 사용하여 만드는 문제로 같은 위치여도 다른 상황일때 판별하는 문제였습니다

​

## 14502번 시작합니다
<img width="900" height="660" alt="image" src="https://github.com/user-attachments/assets/f52975fb-1e82-49d6-9294-9571e46a207a" />

이 문제는 0은 빈칸 1은 벽은 2는 바이러스 가 있는곳으로 바이러스가 0인 곳으로 퍼질 수 있으며, 벽인 1을 빈칸에다가 3개 랜덤 생성이 가능하다고 합니다.  
이때 바이러스는 1인 벽을 넘을 수 없으며,  바이러스가 모두 퍼졌을때 안전지대인 0의 갯수가 최대인 값을 구하는 문제입니다



## 아이디어
바이러스가 그래프에서 한칸씩 이동을 해야하기때문에 bfs를 이용해 줍니다. 
또한, 여기서 벽이 랜덤으로 3개가 확정적으로 추가 생성이 되어야 하므로, 벽이 3개가 만들어지는 모든 경우의 수를 만드는 코드를 작성해야 합니다.


즉, 가능한 모든 경우의 수를 만들땐, 백트레킹을 이용해줘야 하므로, bfs와 백트레킹 두개를 이용하여 문제를 풀어보겠습니다


이 문제를 풀때 int main으로 메인 함수 하나만 만드려고 했지만, 오히려 하나만 만든다면 나중에 코드를 수정할 때 문제가 생겼을 때 수정이 어려울 수 있으므로, 
### 함수를 backtrack, bfs로 나눠서 진행을 했습니다
또한 이 문제를 총 2번을 풀었는데 방법을 2가지 방법으로 나눠서 풀이를 진행해보겠습니다


## 1. 첫번째 방법
​

제일 먼저 문제에서 가장 중요한 bfs부분을 작성해 주려고 합니다 
일반적인 bfs 부분을 작성하는 것처럼 while문을 이용하여 queue가 빌때 까지 pop이랑 push를 해줍니다.

//bfs 부분

<img width="864" height="679" alt="image" src="https://github.com/user-attachments/assets/08e6f1f1-273d-4127-8c5b-713fccf6eb04" />

이때 일반적인 bfs와 다르게 start_x,start_y 같은 시작 부분을 큐에 넣지 않는 이유는
바이러스의 출발점이 여러개 일수도 있으니 bfs함수 에서 하지않고 backtrack함수에서 push와 visited를 true로 해주려고 합니다

이후에 backtrack의 코드를 작성해 줘 봅시다


이전에 벽3개가 다 쌓였을때 계산한 것들이 있기 때문에,
벽이 3개가 다 쌓여 있을때 모든 0을 false로 초기화하고 바이러스들을 다시 queue에 넣어줍니다.

### 이 작업을 안해주면 이전에 벽3개로 나와던 결과값이 다음결과에도 영향을 미칠 수 도 있으니 조심해줍니다!​

//visited를 초기화 및 바이러스 큐에 넣어주는 코드
<img width="757" height="756" alt="image" src="https://github.com/user-attachments/assets/2a3ee9f4-4e6c-489a-b9a7-06b6441247a3" />



이후에 bfs를 호출하여 바이러스가 퍼지지 않은  safezone의 수를 구한다음에 최댓값을 result와 비교를 해서 최종적인 최댓값을 구합니다
//최댓값 구하는 코드
<img width="882" height="525" alt="image" src="https://github.com/user-attachments/assets/81b2d49e-a86b-47ea-b74d-3fe6d4ec46e4" />




​

이제 벽의 갯수가 3개일때 결과를 구하는 코드를 만들었으니 벽을 세우는 코드인  wallcount의 수를 1개씩 랜덤한 위치로 늘려주는 코드를 작성해봅니다

​

//랜덤 위치로 늘려주는 코드
<img width="684" height="478" alt="image" src="https://github.com/user-attachments/assets/72df07c3-4545-4ffc-b71f-3878044b3f56" />

### 백트레킹 특성상 꼭 재귀를 해준다음에 다시 초기화 시켜주는 코드야 있어야합니다.





​​위에 bfs와 backtrack 함수를 모두 이용하여 살을 붙혀서 코드를 작성하면 다음과 같습니다
```cpp
#include <iostream>

#include <queue>

#include <algorithm>

using namespace std;

​

int N, M;

int result = 0;

int dy[4] = {0, 0, -1, 1};

int dx[4] = {-1, 1, 0, 0};

int graph[8][8];

bool visited[8][8];

queue<pair<int, int>> q;

​

void bfs() {

    while (!q.empty()) {

        auto [y, x] = q.front();

        q.pop();

​

        for (int i = 0; i < 4; i++) {

            int ny = y + dy[i];

            int nx = x + dx[i];

​

            // 경계 체크 (nx<M, ny<N로 수정)

            if (nx >= 0 && nx < M && ny >= 0 && ny < N)

                if (!visited[ny][nx] && graph[ny][nx] == 0) {

                    visited[ny][nx] = true;

                    q.push({ny, nx});

                }

        }

    }

}

​

void backtrack(int wallcount) {

    if (wallcount == 3) {

        // 방문 초기화

        for (int i = 0; i < N; i++)

            for (int j = 0; j < M; j++)

                visited[i][j] = false;

​

        // 바이러스 큐 초기화

        // (q는 이전 bfs 끝나면 비어있지만 명시적으로는 비우지 않아도 됨)

        for (int i = 0; i < N; i++) {

            for (int j = 0; j < M; j++) {

                if (graph[i][j] == 2) {

                    q.push({i, j});      // 괄호 오타 수정

                    visited[i][j] = true;

                }

            }

        }

​

        bfs();

​

        int safezone = 0;

        for (int i = 0; i < N; i++)

            for (int j = 0; j < M; j++)

                if (graph[i][j] == 0 && !visited[i][j])

                    safezone++;

​

        result = max(result, safezone);

        return;

    }

​

    // 벽 후보: visited 보지 말고 graph==0만

    for (int i = 0; i < N; i++) {

        for (int j = 0; j < M; j++) {

            if (graph[i][j] == 0) {

                graph[i][j] = 1;

                backtrack(wallcount + 1);  // 증감 방식 수정

                graph[i][j] = 0;

            }

        }

    }

}

​

int main() {

    cin >> N >> M;

​

    for (int i = 0; i < N; i++)

        for (int j = 0; j < M; j++)

            cin >> graph[i][j];

​

    backtrack(0);

​

    cout << result << '\n';  // 출력 오타 수정

}

```






## 두번째 방법

​

두번째로 쓰는 방법은 graph안에서 2의 위치를 따로 vector안에 저장을 하고 
그 vector를 이용해서 bfs에 넣어서 여러번 돌리는 방법을 사용했습니다. 


다만, 이 풀이법은 bfs를 여러번 사용해 줘야하기 때문에 

​

​

### bfs를 사용하기 전에 bfs 안에 있는 큐들을 모두 없애는 과정을 필수로 해줘야합니다​
//bfs 과정
<img width="759" height="796" alt="image" src="https://github.com/user-attachments/assets/2822ee0e-318f-4812-9229-7d66cbb9ab11" />


이후에 backtrack 함수 안에서 첫번째 풀이와  마찬가지로 모든 visited값을 false로 초기화하고 2인 부분들을 모두 bfs를 돌려야 합니다
### 즉, 이말은 graph에서 미리 2인부분을 찾아서 저장한다음에 그 위치를 bfs에 넣어야 한다는 뜻 입니다.​

이후에 sum이라는 것을 만드는데, 이는 

​

### 전체크기-(벽의갯수+새로 만든 벽의 갯수(3개)+감염된 수)  입니다.

이렇게 하면 감염되지 않은 안전지대의 총갯수를 구할 수 있습니다
이 sum의 값과 result값의 최댓값을 result에 저장해봅니다


<img width="792" height="1008" alt="image" src="https://github.com/user-attachments/assets/0d69a719-f7e5-4d7a-b32e-e34af4a536d8" />

메인함수에서 findtwo라는 벡터안에 graph의 값이 2인 위치를 모두 넣어줍니다.
이후에 코드를 완성 시키면 이렇게 나옵니다

```cpp
#include <iostream>

#include <queue>

#include <algorithm>

#include <vector>

using namespace std;

​

int N, M;

int graph[100][100];

bool visited[100][100];

queue<pair<int, int>> q;

vector<pair<int, int>> findtwo;

int result = 0;

int tempcount = 0;

int numcount = 0;

​

void Find(int x, int y) {

    if (x > 0 && !visited[x - 1][y] && graph[x - 1][y] == 0) {

        visited[x - 1][y] = true;

        q.push({x - 1, y});

        numcount++;

    }

    if (x < N - 1 && !visited[x + 1][y] && graph[x + 1][y] == 0) {

        visited[x + 1][y] = true;

        q.push({x + 1, y});

        numcount++;

    }

​

    if (y > 0 && !visited[x][y - 1] && graph[x][y - 1] == 0) {

        visited[x][y - 1] = true;

        q.push({x, y - 1});

        numcount++;

    }

​

    if (y < M - 1 && !visited[x][y + 1] && graph[x][y + 1] == 0) {

        visited[x][y + 1] = true;

        q.push({x, y + 1});

        numcount++;

    }

}

​

void BFS(int start_x, int start_y) {

    while (!q.empty()) q.pop(); // ← 여기서 큐 초기화

    q.push({start_x, start_y});

    visited[start_x][start_y] = true;

    while (!q.empty()) {

        pair<int, int> cur = q.front();

        q.pop();

        Find(cur.first, cur.second);

    }

}

void backtrack(int idx, int cnt) {

    if (cnt == 3) {

        // visited 초기화

        for (int i = 0; i < N; i++)

            for (int j = 0; j < M; j++)

                visited[i][j] = false;

        numcount = 0;

        for (int i = 0; i < findtwo.size(); i++) {

            BFS(findtwo[i].first, findtwo[i].second);

        }

        

int sum = N * M - (tempcount + numcount + 3);

        result = max(result, sum);

        return;

    }

    for (int i = idx; i < N * M; i++) {

        int r = i / M;

        int c = i % M;

        if (graph[r][c] == 0) {

            graph[r][c] = 1;

            backtrack(i + 1, cnt + 1);

            graph[r][c] = 0;

        }

    }

}

​

int main() {

    cin >> N >> M;

    for (int i = 0; i < N; i++) {

        for (int j = 0; j < M; j++) {

            cin >> graph[i][j];

            if (graph[i][j] == 2) {

                findtwo.push_back({i, j});

                tempcount++;

            }

            if (graph[i][j] == 1) {

                tempcount++;

            }

        }

    }

    backtrack(0, 0);

    cout << result;

    return 0;

}
```


### 이 두 코드의 가장 큰 차이점은 바이러스가 여러곳에 존재할때 퍼지는 곳이 동시에 퍼지냐 아니면 한곳이 끝나고나서 다른 곳이 퍼지냐 차이입니다​

즉, 동시에 큐에 들어가냐 아니냐 차이라고 생각할 수 있습니다.

다만, 개인적인 의견이지만, 두번째 방법보단 첫번째 방법이 조금더 나중에 시간을 확인하거나 코드를 추가할때 용이해 보입니다.

​

 

이번 문제 같은 경우에는 단순 bfs만 사용하는것이 아니라 
bfs와 조합을 이용하여 푸는 문제로 이전 문제보다 난이도가 살짝 올라갔습니다!

​
물론, 이 두 코드 모두 백준에 제출을 하면
<img width="893" height="58" alt="image" src="https://github.com/user-attachments/assets/1e049c8e-fbc1-4553-82ff-afe90bba5ae3" />
첫 번째 코드


<img width="898" height="62" alt="image" src="https://github.com/user-attachments/assets/eb234d99-dfbb-445b-afff-31812d997d23" />
두번째 코드

두개 모두 정답이 나옵니다
​
## 실수 한점
​



while (T--)// T시간 지난다는거를 이런식으로 쓰지말자
애초에 이건 T시간을 쓰겠다는 의미가 아닙니다. 같은 시간 즉, 같은 레벨에도 2~3개 혹은 그 이상의 갯수가 있을텐데 이건 그냥 pop();을 T번 하겠다는 완전히 다른 의미입니다

​

#### * visited 초기화 하기!
두 방법에서 모두 visited를 초기화 하는것을 하지 않았습니다. 
백트레킹을 할때 벽이 3개 세워져있을때 마다 bfs를 돌리는데 bfs를 돌릴때 마다 이전에 방문이 된것들은 visited가 true일 테니, 꼭 항상 false로 초기화를 시켜줘야 합니다.

​​
#### * 두 번째 코드에서 bfs​내부 queue를 비워주는 코드 작성이 필수
첫번째 방법에서는 이 실수가 끝이지만, 두번째 코드에서는 bfs내부에 queue를 모두 비워주는 코드를 작성했어야 합니다.
이 역시 코드를 넣지않아서 쓰레기 값이 들어가있었습니다
​
