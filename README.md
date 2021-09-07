# Jacob Daleske
#### My Parents house in Capitan, New Mexico
My Parenst live in a very small house in New Mexico. **Even though I am not a fan of the state itself**, seeing the mountains when you wake up is such a beautiful experience. There, I feel like I have **no stress** to bother me. I always get the time to at least work on one of my father's many project cars he has laying around. Every time I visit, I never want to leave.
---
## Directions to my Favorite Place:
1. Travel South on US-71
2. Take US-59 S to KS-4
3. Follow until I-335 S
4. Follow unitl US-54 W
5. Follow until I-40 W
6. Hop back onto US-54 W
7. Take US-380 E until Capitan, New Mexico

##### Items needed:
* Vehicle, you will be driving close to 14 hours
* At least one person to have deep philisophical conversations with
* At least $500 so you can buy snacks, gas, and other stuff
* Enjoy your stay

**[Learn more About Me](AboutMe.md)**
---
### Best Foods/Drinks In my Opinion

Have you ever been sitting there wondering what to eat/drink? I know I have, and that's why I created this table to help people choose what they should eat or drink!

Food and Drink Table
| Food/Drink | Location | Price |
| --- | --- | --- |
| Dr.Pepper | Basically Anywhere | $2 |
| Pizza | Walmart/Hyvee | $5 |
| Water | Basically Anywhere | $1 |
| Quesadilla | Taco Bell | $5 - $7 |
---
### My Favorite Quotes
> My Fellow Americans, ask not what your country can do for you, ask what you can do for your country.
>> *John F Kennedy*

> At the end of the day, just know I tried.
>> *Robert Daleske*

---
### Lowest Common Ancestor
>> In graph theory and computer science, the lowest common ancestor (LCA) of two nodes v and w in a tree or directed acyclic graph (DAG) T is the lowest (i.e. deepest) node that has both v and w as descendants, where we define each node to be a descendant of itself (so if v has a direct connection from w, w is the lowest common ancestor).
>> [Lowest Common Ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor)

###### Code
```
struct LCA {
    vector<int> height, euler, first, segtree;
    vector<bool> visited;
    int n;

    LCA(vector<vector<int>> &adj, int root = 0) {
        n = adj.size();
        height.resize(n);
        first.resize(n);
        euler.reserve(n * 2);
        visited.assign(n, false);
        dfs(adj, root);
        int m = euler.size();
        segtree.resize(m * 4);
        build(1, 0, m - 1);
    }

    void dfs(vector<vector<int>> &adj, int node, int h = 0) {
        visited[node] = true;
        height[node] = h;
        first[node] = euler.size();
        euler.push_back(node);
        for (auto to : adj[node]) {
            if (!visited[to]) {
                dfs(adj, to, h + 1);
                euler.push_back(node);
            }
        }
    }

    void build(int node, int b, int e) {
        if (b == e) {
            segtree[node] = euler[b];
        } else {
            int mid = (b + e) / 2;
            build(node << 1, b, mid);
            build(node << 1 | 1, mid + 1, e);
            int l = segtree[node << 1], r = segtree[node << 1 | 1];
            segtree[node] = (height[l] < height[r]) ? l : r;
        }
    }

    int query(int node, int b, int e, int L, int R) {
        if (b > R || e < L)
            return -1;
        if (b >= L && e <= R)
            return segtree[node];
        int mid = (b + e) >> 1;

        int left = query(node << 1, b, mid, L, R);
        int right = query(node << 1 | 1, mid + 1, e, L, R);
        if (left == -1) return right;
        if (right == -1) return left;
        return height[left] < height[right] ? left : right;
    }

    int lca(int u, int v) {
        int left = first[u], right = first[v];
        if (left > right)
            swap(left, right);
        return query(1, 0, euler.size() - 1, left, right);
    }
};
```
[Code From CP-algorithms](https://cp-algorithms.com/graph/lca.html)
