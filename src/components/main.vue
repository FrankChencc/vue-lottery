<template>
    <div class="page-lottery">
        <div class="page-lottery-wapper">
            <div class="lottery-wapper">
                <div class="times">{{lotteryConf.times}}</div>
                <lottery class="lottery" :reward-info="reward" :lottery-conf="lotteryConf" @notify="alertPrize" @getPrize="getPrize" @error="alertError"></lottery>
            </div>
        </div>
    </div>
</template>
<script>
    import Lottery from "./Lottery"
    export default {
        name: 'PageLottery',
        components: {
            'lottery': Lottery
        },
        data () {
            return {
                reward: [{
                    name: "流量券1",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券2",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券3",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券4",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券5",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券6",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券7",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                },{
                    name: "流量券8",
                    icon: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514183131932&di=d7e79e906b42fa005ffe30a0725d7167&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2106047825%2C104618452%26fm%3D214%26gp%3D0.jpg"
                }],
                lotteryConf: {
                    // 初始转动速度
                    speed: 60,
                    // 抽奖次数
                    times: 1,
                    // 转动基本次数：即至少需要转动多少次再进入抽奖环节
                    cycle: 40,
                    // 中奖物品的数组index
                    prize: null,
                    // 信息是否完整： 1完整，0不完整
                    perfectInfo: null
                }
            }
        },
        methods: {
            getInfo(){
                let me = this;
                setTimeout(function() {
                    me.isShowLoading = false;
                    me.lotteryConf.times = 1;
                    me.lotteryConf.perfectInfo = 1;
                }, 1000);
            },
            // 获取中奖信息
            getPrize: function() {
                let me = this;
                // 中奖信息
                setTimeout(function() {
                    me.lotteryConf.prize = 1;
                }, 3000);
            },
            // 成功提示信息
            alertPrize: function() {
                var me = this;
                me.lotteryConf.times --;
                // 获奖通知
                window.alert('恭喜获得' + (me.lotteryConf.prize + 1) + '号奖品')
            },
            // 错误提示信息
            alertError: function() {
                var me = this;
                // 信息缺失
                if (me.lotteryConf.perfectInfo === 0) {
                    // 信息缺失
                    window.alert('信息不全不能抽奖')
                } else if (me.lotteryConf.times === 0) {
                    // 重复抽奖
                    window.alert('重复抽奖')
                } else {
                    // 服务器异常
                    window.alert('服务器异常')
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .page-lottery {
        background-color: #fee7c6;   
        .page-lottery-wapper {
            .lottery-wapper {
                margin: 0 auto;
                width: 716px;
                height: 789px;
                background: url('../assets/lottery-bg.png') no-repeat;
                background-size: 100%;
                .times {
                    text-align: center;
                    font-size: 64px;
                    font-weight: 900;
                    color: #bc3ea6;
                    -webkit-text-stroke: 4px #fff;
                    text-shadow:#fff 2px 0 0,#fff 0 2px 0,#fff -2px 0 0,#fff 0 -2px 0;
                    margin-top: -10px;
                }
                .lottery {
                    padding-left: 70px;
                    padding-top: 56px;
                }
            }
        }
    }
</style>