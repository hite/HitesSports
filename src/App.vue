<template>
    <div id="app">
        <img class="w-bg" src="./assets/caochang.png" />
        <div class="w-container">
            <input type="button" value="start" class="w-triggler" v-on:click="start" />
            <div class="w-player" v-for="user in users" v-bind:key="user.no">
                <img class="avatar" :src="user.avatar" />
                <div style="height:12px;" />
                <label class="username">{{user.name}}</label>
            </div>
        </div>
        <div class="w-billboard">
            <span v-if="rankData.length === 0">虚位以待...</span>
            <div class="w-rank-item" v-for="rank in rankData" v-bind:key="rank.no">
                <div class="rank-item-no">{{rank.no}}</div>
                <img class="avatar" :src="rank.avatar" />
                <label class="username">{{rank.username}}</label>
            </div>
        </div>
    </div>
</template>

<script>
import anime from "animejs/lib/anime.es.js";
window.anime = anime;

const kUserNames = [
    "王*亮",
    "周*强",
    "陈*奇",
    "马*军",
    "汪*飞",
    "胡*伍",
    "胡*杰",
    "裘*云",
    "吕*如",
    "张*杰",
    "丁*鑫",
    "张*蕾",
    "纪*策",
    "戴*峰",
    "徐*俊",
    "沈*俊",
    "王*君",
    "闫*萍",
    "陈*盼",
    "黄*凯",
    "朱*利",
    "何*捷",
    "黄*壮",
    "朱*丞",
    "王*涛",
    "江*帆"
];

var objs = kUserNames.map((value, index) => {
    return {
        no: index,
        name: value,
        avatar: require("./assets/avatar/user_" + (index % 17) + ".png")
    };
});
console.log(objs);

function animateEle(_el, far, complete) {
    // y=ax^2+bx+c
    const totalHeight = document.body.clientHeight;
    const totalWidth = document.body.clientWidth;

    // 计算 x，y 轴的数据；
    var translateX = [],
        translateY = [];
    // 起始和步进
    const startX = 60,
        startY = 100;
    let stepX = 40;

    var rank = 0;
    for (var i = 0; ; i++) {
        // console.log('i = ' + i)
        var x = startX + i * stepX;
        var duration = 10 + 90 * far;
        var xObject = { value: x, duration: duration };
        var y = (0.0004 + far * 0.001) * Math.pow(x, 2) + 0.01 * x + 10;
        var yObject = { value: y, duration: duration };
        translateX.push(xObject);
        translateY.push(yObject);
        if (y > totalHeight - startY - 100) {
            console.log("y =" + y);
            console.log("x = " + x);
            rank = x;
            break;
        }
    }
    //
    anime({
        targets: _el,
        translateX: translateX,
        translateY: translateY,
        easing: "linear",
        complete: function(anim) {
            if (typeof complete === "function") {
                complete(rank);
            }
        }
    });
    return rank;
}
export default {
    name: "app",
    data: function() {
        return {
            users: objs,
            rankData: []
        };
    },
    methods: {
        start: function(event) {
            // 清理旧榜单
            this.rankData = [];

            var els = document.querySelectorAll(".w-player");
            var rawRanks = [];

            els.forEach((el, idx) => {
                var far = Math.random();
                console.log(far);
                animateEle(el, far, rank => {
                    rawRanks.push({
                        rank: rank,
                        index: idx
                    });
                    // 排序第一第二
                    var ranks = rawRanks.sort(function(a, b) {
                        return a.rank < b.rank;
                    });
                    // console.log(ranks);
                    // 组装榜单；
                    this.rankData = ranks.map((val, idx) => {
                        return {
                            no: idx + 1,
                            username: kUserNames[val.index],
                            avatar: require("./assets/avatar/user_" +
                                (val.index % 17) +
                                ".png")
                        };
                    });
                });
            });
        }
    }
};
</script>

<style scoped>
.w-bg {
    height: 100%;
    width: 100%;
    position: absolute;
}
.w-triggler {
    width: 100px;
    height: 40px;
    background-color: rgba(255, 255, 255, 0.6);
    border: none;
    border-radius: 10px;
    font-size: 20px;
    position: absolute;
    left: 48%;
    top: 20px;
}
.w-container {
    position: relative;
}

.w-billboard {
    background-color: rgba(255, 255, 255, 0.65);
    position: absolute;
    top: 100px;
    right: 50px;
    bottom: 100px;
    border-radius: 10px;
    width: 170px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
    align-content: flex-start;
    overflow-y: scroll;
}

.w-rank-item {
    display: flex;
    width: 140px;
    height: 50px;
    align-items: center;
    justify-content: space-between;
    padding-bottom: 5px;
}
.rank-item-no {
    height: 20px;
    width: 20px;
    border-radius: 10px;
    background-color: black;
    color: white;
    font-size: 18px;
    text-align: center;
    line-height: 21px;
}

.w-run-lane {
    bottom: 0;
    left: 0;
    bottom: 0;
    right: 0;
    height: 170px;
    background-image: url(./assets/lane@3x.png);
}

.w-player {
    width: 100px;
    height: 80px;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: absolute;
    left: 30px;
    top: 50px;
}
.avatar {
    width: 40px;
    height: 40px;
}
.username {
    background-color: aliceblue;
    color: #2c3e50;
    font-size: 16px;
    font-weight: 500;
    border-radius: 5px;
    padding: 4px;
}

.w-rank-item .username {
    background-color: transparent;
    color: black;
    width: 60px;
}
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    width: 100%;
    height: 100%;
    overflow: hidden;
}
</style>
