# 希德的运动会

本质是一个抽奖的应用，用途在一堆人里选出 top n 个人来。
它和普通的抽奖系统的区别在于；

- 好玩，每个都有一个头像。抽奖过程是一个跳远的过程
- 高效，每次选出 n 人，当事人空缺时，可选择后补

## 玩法
1. 去修改源代码里的人员名单，
```javascript
const kUserNames = [
    "柳*亮",
    "周*强"
    ]
```
2. 点击屏幕上的 start 按钮，开始一次尝试；
