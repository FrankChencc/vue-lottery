<template>
    <div class="box">
        <div class="item" v-for="(item, index) in rewardInfo" v-if="index < 4">
            <div class="mask" v-show="isRuning && changeIndex(index) != lightIndex"></div>
            <img :src="item.icon" alt="" class="logo">
            <p class="name">{{item.name}}</p>
        </div>
        <div class="click-btn" :class="{'start-btn': lotteryConf.times != 0 && !isRuning}" @click="startLottery">
            <div class="desc">{{lotteryConf.times|totext}}</div>
        </div>
        <div class="item" v-for="(item, index) in rewardInfo" v-if="index > 3 && index < 8">
            <div class="mask" v-show="isRuning && changeIndex(index) != lightIndex"></div>
            <img :src="item.icon" alt="" class="logo">
            <p class="name">{{item.name}}</p>
        </div>
    </div>
</template>
<script>
    export default  {
        name: 'lottery',
        props: ['rewardInfo', 'lotteryConf'],
        data () {
            return {
                // 当前位置索引
                lightIndex: -1,
                // 总共奖品数
                count: 8,
                // 转圈的计时器
                timer: null,
                // 当前转动次数
                times: 0,
                // 运行标志位
                isRuning: false,
                // 运行速度
                speed: this.lotteryConf.speed
            }
        },
        filters: {
            totext (value){
                let res;
                switch (value) {
                    case 0:
                    case '0':
                        res = '下周再来';
                        break;
                    case 1:
                    case '1':
                        res = '开始抽奖';
                        break;
                    default:
                        res = '开始抽奖';
                        break;
                };
                return res;
            }
        },
        methods: {
            // 将数组的索引改变为转盘的索引
            changeIndex(index) {
                if (index < 3) {
                    return index;
                } else if (index == 3) {
                    return 7;
                } else if (index == 4){
                    return 3;
                } else if (index > 8){
                    return 0
                } else {
                    return 11 - index;
                }

            },
            //索引增加
            addIndex: function() {
                let me = this;
                this.lightIndex++;
                if(this.lightIndex > this.count - 1) {
                    me.lightIndex = 0;
                }
            },
            //运动
            rotate: function() {
                let me = this;
                clearTimeout(me.timer);
                if(typeof(me.rotate) == 'function') {
                    me.timer = setTimeout(function() {
                        me.rotate();
                    }, me.speed);
                }
                // 判断是否符合停止条件
                me.judge();

                me.addIndex();
                me.times++;
            },
            //判断加减速和是否停止
            judge: function() {
                let me = this;
                // 完成条件：转动次数大于基本转动次数
                if(this.times > this.lotteryConf.cycle) {
                    // 后端返回出错条件：奖品索引不存在
                    if(this.lotteryConf.prize === null) {
                        clearTimeout(me.timer);
                        me.timer = null;
                        me.times = 0;
                        me.speed = this.lotteryConf.speed;
                        me.lightIndex = -1;
                        me.isRuning = false;

                        // 触发错误通知函数
                        me.$emit('error');

                    }
                    // 获奖条件：当前索引等于奖品数
                    if(this.lightIndex == this.changeIndex(this.lotteryConf.prize)) {
                        clearTimeout(me.timer);
                        me.timer = null;
                        me.times = 0;
                        me.speed = this.lotteryConf.speed;
                        me.lightIndex = -1;
                        me.isRuning = false;

                        // 触发中奖通知函数
                        me.$emit('notify');
                    }
                }
                
                //速度控制：快接近 减速
                if((this.times > this.lotteryConf.cycle - this.count / 2) && (this.times < this.lotteryConf.cycle + this.count / 2)) {
                    this.speed += 80;
                } else {
                    this.speed += 3;
                }
            },
            //中奖礼品
            startLottery: function() {
                let me = this;
                // 抽奖次数不足
                if (me.lotteryConf.times < 1) {
                    return false;
                }
                // 抽奖信息不完整
                if (me.lotteryConf.perfectInfo === 0) {
                    // 触发错误通知函数
                    me.$emit('error');
                    return false;
                }
                // 正在运行中
                if (me.isRuning) {
                    return false;
                };
                me.isRuning = true;
                me.rotate();
                me.$emit('getPrize');
            }

        }
    }
</script>
<style lang="scss" scoped>
    .box {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        width: 570px;
        height: 580px;
        .item {
            width: 183px;
            height: 187px;
            background: url('../assets/reward-light.png') no-repeat;
            background-size: 100%;
            position: relative;
            .logo {
                width: 140px;
                height: 110px;
                margin: 0 auto;
                margin-top: 10px;
                display:block;
            }
            .name {
                text-align: center;
                color: #5c1c0a;
                font-size: 25px;
                margin-top: 6px;
            }
            .mask {
                top: 0;
                left: 0;
                position: absolute;
                background-color: rgba(16,16,16,0.4);
                border-radius: 28px;
                width: 183px;
                height: 187px;
            }
        }
        .click-btn {
            width: 179px;
            height: 179px;
            background: url('../assets/btn-light.png') no-repeat;
            background-size: 179px 179px;
            .desc {
                color: #fff;
                font-size: 50px;
                width: 120px;
                line-height: 50px;
                text-align: center;
                margin: 0 auto;
                margin-top: 40px;
            }
        }
        .start-btn:active {
            background: url('../assets/btn-dark.png') no-repeat;
            background-size: 179px 179px;
        }
    }
</style>
