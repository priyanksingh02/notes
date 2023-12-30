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

### Trie

#### Alphabets trie using pointers

```cpp

```

#### Trie for max XOR

```cpp
// using pointers
struct node {
    vector<node*> child {nullptr, nullptr};
};

void insert(node * root, int x) {
    int test = 8;
    for(int i = 31; i >= 0; --i) {
        int pos = (x & (1<<i)) > 0;
        if(!root->child[pos]) root->child[pos] = new node();
        root = root->child[pos];
    }
}

int txor(node* root, int x) {
    int ans = 0;
    for(int i = 31; i >= 0; --i) {
        int pos = (x & (1 << i)) > 0;
        pos ^= 1;
        if(root->child[pos]) {
            root = root->child[pos];
            ans |= (1<<i);
        }
        else {
            root = root->child[1-pos];
        }
    }
    return ans;
}

int maxor(node * a, node * b, int pos) {
    if(!a or !b) {
        // cout << "Unknown" << pos << " " <<  a << " " << b << endl;
        return 0;
    }
    if(pos < 0) return 0;
    int ans = 0;
    if((a->child[0] and b->child[1]) or (a->child[1] and b->child[0]))
    {
        ans |= (1<<pos);
        ans |= max(maxor(a->child[0], b->child[1], pos-1), maxor(a->child[1], b->child[0], pos-1));
    }
    else {
        if(a->child[0] and b->child[0]) {
            ans |= maxor(a->child[0], b->child[0], pos-1);
        }
        else {
            ans |= maxor(a->child[1], b->child[1], pos-1);
        }
    }
    return ans;
}

class Solution {
    node root;
public:
    int findMaximumXOR(vector<int>& nums) {
        for(int x: nums) {
            insert(&root, x);
        }
        return maxor(&root, &root, 31);
    }
};
```

```cpp
// using global arrays

const int max_n = 500005 * 20;
int tr[max_n][2], p_idx;

void insert(int x) {
    int p = 1;
    for(int i = 31; i>=0; --i) {
        int idx = (x >> i) & 1 ;
        if(!tr[p][idx]) tr[p][idx] = ++p_idx;
        p = tr[p][idx];
    }
}

int query(int x) {
    int ans = 0, p = 1;
    for(int i = 31; i >= 0; --i) {
        int idx = !((x >> i) & 1);
        if(tr[p][idx]) ans |= (1<<i), p = tr[p][idx];
        else p = tr[p][!idx];
    }
    return ans;
}

class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        memset(tr, 0, sizeof(tr));
        p_idx = 1;
        for(int x: nums) {
            insert(x);
        }
        int ans = 0;
        for(int x: nums) {
            ans = max(ans, query(x));
        }
        return ans;
    }
};
```

```cpp
// source for above array trick
const int N = 50005 * 20;

int tr[N][2], idx, c[N];

void clr() {
	for (int i = 1; i <= idx; i++) tr[i][0] = tr[i][1] = c[i] = 0;
	idx = 1;
}

void ins(int x, int k) {
    int p = 1;
    for (int i = 19; ~i; i--) {
        int ch = x >> i & 1;
        if(!tr[p][ch]) tr[p][ch] = ++idx;
        p = tr[p][ch];
        c[p] += k;
    }
}
int get(int x) {
    int p = 1, res = 0;
    for (int i = 19; ~i; i--) {
        int ch = !(x >> i & 1);
        if(tr[p][ch] && c[tr[p][ch]]) res |= 1 << i, p = tr[p][ch];
        else p = tr[p][!ch];
    }
    return res;
}

class Solution {
public:

    int maximumStrongPairXor(vector<int>& a) {
    	idx = 1;
        sort(a.begin(), a.end());
        int n = a.size();
        int ans = 0;
        for (int i = 0, j = 0; i < n; i++) {
        	ins(a[i], 1); // 1 means adding to trie
        	while (a[i] - a[j] > a[j]) {
        		ins(a[j], -1); // -1 means removing from trie
        		j++;
        	}
        	chkMax(ans, get(a[i]));

        }
        clr();
    	return ans;
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
