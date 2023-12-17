# Notes

## Data Structures

### Disjoint Set

```cpp

#include<numeric> // for iota

struct dset {
    vector<int> parent, rank;
    dset(int sz) {
        parent.resize(sz);
        iota(parent.begin(), parent.end(), 0);
        rank.resize(sz,0);
    }
    int find(int x) {
        return (parent[x] == x)? x : parent[x] = find(parent[x]);
    }
    void merge(int a, int b) {
        int pa = find(a), pb = find(b);
        if(pa == pb) return;
        if(rank[pb] > rank[pa]) parent[pa] = pb;
        else {
            parent[pb] = pa;
            if(rank[pa]== rank[pb]) rank[pa]++;
        }
    }
};

```

## Algorithms

### Dijkstra 

#### using `set`

```cpp

vector<int> dijkstra (vector<vector<pair<int,int>>> & adjlist, int src) {
    int n = adjlist.size();
    const int inf = 1e9;
    vector<int> dist (n, inf);
    dist[src] = 0;
    set<pair<int,int>> s;
    s.insert({dist[src], src});
    while(!s.empty()) {
        auto u = s.begin()->second;
        s.erase(s.begin());
        for(auto v : adjlist[u]) {
            if(dist[v.first] > dist[u] + v.second) {
                if(dist[v.first] != inf) s.erase({dist[v.first], v.first});
                dist[v.first] = dist[u] + v.second;
                s.insert({dist[v.first], v.first});
            }
        }
    }
    return dist;
}

```

### DFS

```cpp
vector<vector<int>> adjlist;
int n = adjlist.size();
vector<bool> visited(n, false);
void dfs(int src) {
    visited[src] = true;
    for(int dest: adjlist[src])
        if(!visited[dest])
            dfs(dest);
}
```

## Tricks

### 2D directions

```cpp
// 4 directions
int dir[] = {1,0,-1,0,1};
for(int i = 0; i < 4; ++i) {
    int new_x = x + dir[i];
    int new_y = y + dir[i+1];
}

// 8 directions
for(int i = -1; i <= 1; ++i) {
    for(int j = -1; j <= 1; j++) {
        int new_x = x + i;
        int new_y = y + j;
    }
}
```
