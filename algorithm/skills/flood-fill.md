---
description: Flood Fill 알고리즘에 대해 알아보고, 이를 활용한 문제 해결 방법을 살펴봅니다.
---

# Flood Fill

## Flood Fill 이란?

***

Flood Fill은 [bfs.md](bfs.md "mention")와 유사한 방식으로 동작하며, 좌표 환경(지도)에서 특정 좌표를 기준으로 퍼져나가 듯 탐색하는 기법입니다.

흔히 색칠 알고리즘이라고도 하며, 지도에서의 BFS 활용이 기본적인 구조이기에 [dx-dy.md](dx-dy.md "mention")테크닉이 동반되는 경우가 많습니다.

{% hint style="info" %}
Flood Fill 은 BFS + Dx Dy + DAT 가 혼합된 알고리즘입니다.
{% endhint %}

### Flood Fill 구조

기본적인 BFS 에 2차원 좌표 평면에서의 진행을 위한 dx dy 테크닉을 적용하고, 진행 과정을 DAT 배열을 통해 기록합니다.

```cpp
int DAT[N][M];

int dy[4] = {1, 0, -1, 0};
int dx[4] = {0, 1, 0, -1};

void BFS() {
    queue<pair<int, int>> q; // (y, x)
    q.push(make_pair(0, 0)); // (0, 0)에서 시작한다고 가정
    
    while(!q.empty()) { // BFS
        pair<int, int> now = q.front();
        q.pop();
        
        for(int i = 0; i < 4; i++) { // Dx Dy
            int newY = now.first + dy[i];
            int newX = now.second + dx[i];
            
            if(newY < 0 || newY >= N || newX < 0 || newX >= M
                || DAT[newY][newX] > 0) continue;
            
            DAT[newY][newX] = DAT[now.first][now.second] + 1; // DAT
            q.push(make_pair(newY, newX));
        }
    }
}
```

## Flood Fill 활용

***

Flood Fill은 2차원 평면에서 BFS 진행과 유사하기 때문에 기본적으로 BFS의 활용도와 특징을 가집니다.

아래는 대표적으로 Flood Fill을 활용하는 방법입니다.

### 2가지 유형으로 겹치지 않게 배치하기

Flood Fill은 기본적으로 Layer를 기준으로 퍼져나가듯 색칠하는 알고리즘입니다. 이를 활용하여 홀수 Layer와 짝수 Layer의 색깔을 다르게 한다면 2가지 색으로 겹치지 않게 색칠이 가능합니다.

{% hint style="warning" %}
단, [#depth](bfs.md#depth "mention")를 통해 각 Layer의 변화를 감지해야 합니다.
{% endhint %}

### 최단 거리 구하기

기존 BFS와 동일하게 최단 거리를 구할 때 사용 가능합니다. 이는 사실상 BFS를 통해 최단거리를 구한다는 말과 동일합니다.



## Summary

***

* Flood Fill은 2차원 좌표 평면에서 특정 좌표를 기준으로 퍼져나가 듯 탐색하는 기법입니다.
* 색칠 알고리즘이라고도 부르며, 각 Layer를 기준으로 펴져나가 듯 탐색됩니다.
* 기본적으로 BFS를 사용하며, Dx Dy, DAT 기법을 동시에 활용합니다.
