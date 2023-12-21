# Notes

## Important links

* [Track live and upcoming contests](https://clist.by/)
* [Must Do Coding Questions for Companies like Amazon, Microsoft, Adobe](https://www.geeksforgeeks.org/must-do-coding-questions-for-companies-like-amazon-microsoft-adobe/)
* [Awesome competitive programming](https://github.com/lnishan/awesome-competitive-programming)
* [Competitive Programmer’s Handbook](https://cses.fi/book/book.pdf)
* [Errichto · GitHub - For competitive programming template](https://github.com/Errichto)
* [TopCoder - Competitive Programming - Tutorials #articles](https://www.topcoder.com/community/competitive-programming/tutorials/)
* [CP-Algorithms](https://cp-algorithms.com/)
* [MAXimal :: algo](https://e-maxx.ru/algo/)
* [All the good tutorials found for Competitive Programming - Codeforces](https://codeforces.com/blog/entry/57282)
* [Stanford ICPC - Standford Univesity](https://cs.stanford.edu/group/acm/resources.html)
* [Stanford University ICPC Team Notebook (2015-16)](https://cs.stanford.edu/group/acm/oldsite/SLPC/notebook.pdf)
* [A comprehensive guide to Competitive Programming](https://codeforces.com/group/ZEXri9keRR/blog)
* [Competitive Programming Book](https://cpbook.net/)
* [GitHub - helloproclub/competitive-programming-cheat-sheet: C++ Cheat Sheet for ACM ICPC](https://github.com/helloproclub/competitive-programming-cheat-sheet)
* [Bit Twiddling Hacks](https://graphics.stanford.edu/~seander/bithacks.html)
* [Competitive Programming Articles](https://ncduy0303.github.io/Competitive-Programming/)

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
        if(i == 0 and j == 0) continue;
        int new_x = x + i;
        int new_y = y + j;
    }
}
```
