# 朋友圈（中等）
# 题目描述
![截屏2019-11-21上午10.24.26.png](https://pic.leetcode-cn.com/6d8f28ad436e564d3d88a51da96e28e705ab277268ffe74c12f7af3d968a2861-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8A%E5%8D%8810.24.26.png)
![截屏2019-11-21上午10.24.35.png](https://pic.leetcode-cn.com/b77ec1a456b14ba6c5ed79d4cd7d76c9deb0d153a742d0e87936cf3ff316ddd4-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8A%E5%8D%8810.24.35.png)
# 题目地址
<https://leetcode-cn.com/problems/friend-circles/>
## 解法三
  + ![截屏2019-11-21下午4.29.50.png](https://pic.leetcode-cn.com/9a2a9ed322e0f8cb4652b1412f5113cdb02e0c28a5e85f406e99c157f49b4e88-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%884.29.50.png)
## 解法四
  + ![截屏2019-11-21下午5.43.40.png](https://pic.leetcode-cn.com/39b9d266395c00d6ed7f203930c32b7d0c7174c38ff4f7e8108939b31cd8f5e0-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%885.43.40.png)
#### 解法一：DFS求图的连通分量
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)，访问数组的大小
+ 图
  + 将每个学生当中一个顶点，朋友关系组成连接顶点点边，不分出入度
  + 那么整个班级的学生关系就构成了图这种数据结构
+ 连通图
  + 若V(G)中任意两个不同的顶点V(i)和V(j)都连通，即有路径或者有边相连，则称G为连通图(Connected Graph)
  + 对应题中，从任一顶点出发遍历，可以访问到图到所有顶点，即所有两两相连的朋友关系顶点，针对关系顶点重复以上步骤，直到没有未访问的相连的顶点，剩下的顶点就是另一个连通图的，亦即另外一个朋友圈
+ 分析
  + 此题明显为非连通的无向图
    + 即有n个子连通图
  + 任何连通图的连通分量只有一个，即其自身
    + 对应题中的一个人也是自己的朋友圈
  + 非连通的无向图有多个连通分量
    + 对应题中求班级所有人的朋友圈总数
+ DFS
  + 关键：求子连通图的个数，即连通分量
  + 对每个未被访问的顶点进行深度优先搜索，每进入到下一个连通图的搜索结果个数+1
```javascript
/**
 * @param {number[][]} M
 * @return {number}
 */
var findCircleNum = function(M) {
    let n = M.length;
    if(n == 0){
        return 0;
    }
    let count = 0;
    let dfs = (i) => {
        for(let j = 0; j < n;j++){
            if(M[i][j] == 1 && !visited[j]){
                visited[j] = true;
                dfs(j);
            }
        }
    }
    let visited = {};
    for(let i = 0;i < n;i++){
        if(!visited[i]){
            dfs(i);
            count++;
        }
    }
    return count;
};
```
#### 解法二：BFS求图的连通分量
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ DFS是针对一个顶点无限度的追溯与其相连的顶点，直到最后一级
+ BFS是针对每个顶点的邻接顶点访问，再访问这些邻接顶点的连接顶点，一级或一层层的访问
```javascript
/**
 * @param {number[][]} M
 * @return {number}
 */
var findCircleNum = function(M) {
    let n = M.length;
    if(n == 0){
        return 0;
    }
    let count = 0;
    let bfs = (i) => {
        let queue = [i];
        while(queue.length > 0){
            let adjacentPoint = queue.pop();
            for(let j = 0; j < n;j++){
                if(M[adjacentPoint][j] == 1 && !visited[j]){
                    visited[j] = true;
                    queue.push(j);
                }
            }
        }
    }
    let visited = {};
    for(let i = 0;i < n;i++){
        if(!visited[i]){
            bfs(i);
            count++;
        }
    }
    return count;
};
```
___
## ***解法三、解法四思路分析***
+ 并查集
  + 是一种树型的数据结构，用来**处理一些不相交集合(Disjoint Sets)的合并及查询问题**
  + 联合-查找算法(union-find algorithm)，规范了对此数据结构的操作
    + Find：**确定元素属于哪一个子集**
      + 方法是不断向上查找它的根节点
      + 因而可以**被用来确定两个元素是否属于同一子集**的问题
    + Union：**将两个子集合并成同一个集合**
    + 初始化：
      + 每个元素是自己的集合代表，即自己是一个集合，我为自己代言；
        + ![截屏2019-11-21下午10.39.28.png](https://pic.leetcode-cn.com/4da71cd9c2f65f0b473ae4b9b66cce2bf86ace39fd80cd53ca5588edac7faebf-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.28.png) 
      + 设当前元素为**代表**
        + Find(x)返回x所属集合的代表
        + Union(Find(x(1)),Find(x(2)))
          + **按秩合并两个不相交集合**
          + **求连通分量**
+ 求解
  + 并查集森林
    + 是一种**将每一个集合以树表示的数据结构**，**每一个节点保存着它的父节点引用地址**
    + **每个集合的代表即是集合的根节点**
    + **查找**
      + 根据父节点引用向上进行搜索直到root根节点
    + **联合**
      + 将两棵树合并到一起，即将其中一棵树的根连接到另一棵树的根的操作
    + **方法模板**
        ```javascript
        let MakeSet = (x) => {
            parent[x] = x;
        }
        let find = (x) => {
            if(parent[x] == x){
                return x;
            }
            return find(parent[x])
        }
        let union = (x,y) => {
            let rootX = find(x);
            let rootY = find(y);
            if(rootX == rootY){
                return;
            }
            parent[rootX] = rootY;
        }
        ``` 
    + 优化方法
      + **按秩合并**
        + ![截屏2019-11-21下午10.39.38.png](https://pic.leetcode-cn.com/ff5696fcb2fbadfbf42cbe6aeec6acf73618686b062ade33023c37720aa21b88-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.38.png)
        + 这里的**秩**可以理解为**树的深度**
        + **总是将更小的树连接到更大的树上，即改变小根节点到大根节点上**
        + 初始化
          + 单元素大树秩定义为0
        + 当两棵秩相同时，均+1
        + 效率
          + 提升至O(logn)
        + **优化方法模板A**
          ```javascript
          let MakeSet = (x) => {
              parent[x] = x;
              rank[x] = 1;
          }
          let find = (x) => {
              if(parent[x] == x){
                  return x;
              }
              return find(parent[x])
          }
          let union = (x,y) => {
              let rootX = find(x);
              let rootY = find(y);
              if(rank[rootX] > rank[rootY]){
                  parent[rootY] = rootP;
              }else if(rank[rootX] < rank[rootY]){
                  parent[rootX] = rootY;
              }else{
                  parent[rootY] = rootX;
                  rank[rootX]++;
              }
          }
          ``` 
      + **路径压缩**
        + ![截屏2019-11-21下午10.39.44.png](https://pic.leetcode-cn.com/3273f7fc5f5e6e810fafa277d1babf07e259a61fb4fba7355edb06eede4e3eb8-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.44.png)
        + **处于同一路径上的节点都直接连接到根节点上**
        + **优化方法模板B**
        ```javascript
        let MakeSet = (x) => {
            parent[x] = x;
        }
        let find = (x) => {
            while(parent[x] != x){
               parent[x] = Find(parent[x]);
            }
            return parent[x];
        }
        let union = (x,y) => {
            let rootX = find(x);
            let rootY = find(y);
            if(rootX == rootY){
                return;
            }
            parent[rootX] = rootY;
        }
        ``` 
    + 总结：将以上两种优化方法合并，执行时间提升客观
    + 这两种方法的优势互补，同时使用二者的程序每个操作的平均时间仅为
    + $$O(\alpha (n))$$  
    + 实际上，这是渐近最优算法
  + 总结下包含**路径压缩**和**按秩合并**两种优化的最优代码模板来求解并查集问题
    ```javascript
    /**
     * @param {number[][]} M
     * @return {number}
     */
    var findCircleNum = function(M) {
        let n = M.length;
        if(n == 0){
            return 0;
        }
        // 求连通分量
        let count = n;
        // 给每个元素单集建立秩
        let rank = new Array(n);
        let find = (p) => {
            while( p != parent[p]){
                // 路径压缩
                // 区别与上述模版，很巧妙，通过直接将当前节点的父节点直接指向爷爷节点
                parent[p] = parent[parent[p]];
                p = parent[p];
            }
            return p;
        }
        // 查询两个相交集合
        let union = (p,q) => {
            let rootP = find(p);
            let rootQ = find(q);
            // 集合相交则不合并
            if(rootP == rootQ){
                return;
            }
            // 按秩合并
            if(rank[rootP] > rank[rootQ]){
                parent[rootQ] = rootP;
            }else if(rank[rootP] < rank[rootQ]){
                parent[rootP] = rootQ;
            }else{
                parent[rootQ] = rootP;
                // 相同的秩都加1
                rank[rootP]++;
            }
            // 求连通分量，每合并两个集合，即整体减少一个独立集合
            count--;
        }
        let parent = new Array(n);
        for(let i = 0;i < n;i++){
            parent[i] = i;
            // 初始化秩
            rank[i] = 1;
        }
        // 并查集搜索开始
        for(let i = 0;i < n;i++){
            for(let j = 0; j < n;j++){
                if(M[i][j] == 1){
                    union(i,j);
                }
            }
        }
        // 返回连通分量结果
        return count;
    };
    ```
  + 三要素
    + makeSet(s)：建立一个新的并查集，其中包含n个单元素集合
    + unionSet(x,y)：将元素x和元素y所在的集合合并
      + 若不相交则不合并
      + 若相交
        + 按秩合并为优化手段
        + 可用于求连通分量
    + find(x)：
      + 找到元素x所在的集合的代表
        + 路径压缩为优化手段
        + 可用于判断两个元素是否在同一个子集
+ 类似题型
  + [200. 岛屿数量-解法三](https://leetcode-cn.com/problems/number-of-islands/solution/200-dao-yu-shu-liang-by-alexer-660/)
___
#### 解法三：并查集森林 + 路径压缩
```javascript
/**
 * @param {number[][]} M
 * @return {number}
 */
var findCircleNum = function(M) {
    let n = M.length;
    if(n == 0){
        return 0;
    }
    let count = n;
    // 查找
    let find = (p) => {
        while( p != parent[p]){
            // 路径压缩
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
    let union = (p,q) => {
        let rootP = find(p);
        let rootQ = find(q);
        // 集合相交则不合并
        if(rootP == rootQ){
            return;
        }
        // 查找合并
        parent[rootP] = rootQ;
        // 求连通分量，每合并两个集合，即整体减少一个独立集合
        count--;
    }
    let parent = new Array(n);
    for(let i = 0;i < n;i++){
        parent[i] = i;
    }
    for(let i = 0;i < n;i++){
        for(let j = 0; j < n;j++){
            if(M[i][j] == 1){
                union(i,j);
            }
        }
    }
    return count;
};
```
#### 解法四：并查集森林 + 路径压缩 + 按秩合并
```javascript
/**
 * @param {number[][]} M
 * @return {number}
 */
var findCircleNum = function(M) {
    let n = M.length;
    if(n == 0){
        return 0;
    }
    let count = n;
    let rank = new Array(n);
    let find = (p) => {
        while( p != parent[p]){
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
    let union = (p,q) => {
        let rootP = find(p);
        let rootQ = find(q);
        if(rootP == rootQ){
            return;
        }
        if(rank[rootP] > rank[rootQ]){
            parent[rootQ] = rootP;
        }else if(rank[rootP] < rank[rootQ]){
            parent[rootP] = rootQ;
        }else{
            parent[rootQ] = rootP;
            rank[rootP]++;
        }
        count--;
    }
    let parent = new Array(n);
    for(let i = 0;i < n;i++){
        parent[i] = i;
        rank[i] = 1;
    }
    for(let i = 0;i < n;i++){
        for(let j = 0; j < n;j++){
            if(M[i][j] == 1){
                union(i,j);
            }
        }
    }
    return count;
};
```