# 模拟行走机器人（简单）
# 题目描述
![截屏2019-11-04下午3.55.34.png](https://pic.leetcode-cn.com/5a67652abf75e993fb67853ad62637bbcc01b19d4dc2dbc69ad6790fa67ff6b2-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8B%E5%8D%883.55.34.png)
# 题目地址
<https://leetcode-cn.com/problems/walking-robot-simulation/>
#### 解法:
+ 类似题型
+ [200.岛屿数量](https://leetcode-cn.com/problems/number-of-islands/solution/200-dao-yu-shu-liang-by-alexer-660/)
+ 思路
  ![截屏2019-11-06下午7.47.38.png](https://pic.leetcode-cn.com/d804ea6c04e857421056f5914c3c08f2c741b91d75811dab76f96a879bbf45b7-%E6%88%AA%E5%B1%8F2019-11-06%E4%B8%8B%E5%8D%887.47.38.png)
+ 分析
  + 由题意可知，初始化机器人位置为原点(0,0)
  + 初始化坐标系
    + dx = [0,1,0,-1]
    + dy = [1,0,-1,0]
    + 其实就是坐标系点从北部方向顺时针方向存放点坐标的方向向量数组
    + dx和dy一一对应一个坐标点
  + 机器人行走方向
    + (-2):左转，O->W,Wx = (Ox+3)%4
    + (-1):右转，O->E,Wx = (Ox+1)%4
    + 解释
      + 方向只有东西南北四个方向，所以无论怎么走，都不会超出方向向量数组里的取值范围
      + 因此通过取模来循环取对应数组的位置 
    + (x): 机器人不转弯并向前走X个格子
      + 一步一步走，从1开始遍历
      + 判断下一步是否有障碍物
        + 有，继续走
        + 无，进入下一轮
  + 解题技巧
    + 存储障碍物为hash表，提升程序性能    
```javascript
/**
 * @param {number[]} commands
 * @param {number[][]} obstacles
 * @return {number}
 */
var robotSim = function(commands, obstacles) {
    var dx = [0,1,0,-1];
    var dy = [1,0,-1,0];
    var di = 0;
    var endX = 0;
    var endY = 0;
    var result = 0;
    var hashObstacle = {};
    for(var r = 0;r<obstacles.length;r++){
        hashObstacle[obstacles[r][0]+'-'+obstacles[r][1]] = true;
    }
    for(var s = 0;s<commands.length;s++){
        if(commands[s] == -2){
            di = (di+3)%4;
        }else if(commands[s] == -1){
            di = (di+1)%4;
        }else{
            // 每次走一步
            for(var z = 1;z <= commands[s];z++){
                var nextX = endX + dx[di];
                var nextY = endY + dy[di];
                // 判断下一步是否为障碍物
                if(hashObstacle[nextX+'-'+nextY]){
                    break;
                }
                endX = nextX;
                endY = nextY;
                result = Math.max(result,endX*endX+endY*endY);
            }
        }
    }
    return result;
};
```
+ 优化版
  + [思想来源于：641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/solution/641-she-ji-xun-huan-shuang-duan-dui-lie-by-alexer-/) 
  + 即取模改成位运算，提升程序性能 
```javascript
/**
 * @param {number[]} commands
 * @param {number[][]} obstacles
 * @return {number}
 */
var robotSim = function(commands, obstacles) {
    var dx = [0,1,0,-1];
    var dy = [1,0,-1,0];
    var di = 0;
    var endX = 0;
    var endY = 0;
    var result = 0;
    var hashObstacle = {};
    for(var r = 0;r<obstacles.length;r++){
        hashObstacle[obstacles[r][0]+'-'+obstacles[r][1]] = true;
    }
    for(var s = 0;s<commands.length;s++){
        if(commands[s] == -2){
            di = (di+3)&3;
        }else if(commands[s] == -1){
            di = (di+1)&3;
        }else{
            // 每次走一步
            for(var z = 1;z <= commands[s];z++){
                var nextX = endX + dx[di];
                var nextY = endY + dy[di];
                // 判断下一步是否为障碍物
                if(hashObstacle[nextX+'-'+nextY]){
                    break;
                }
                endX = nextX;
                endY = nextY;
                result = Math.max(result,endX*endX+endY*endY);
            }
        }
    }
    return result;
};
```