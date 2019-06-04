<template>
    <div class='game-operating'>
        <div class='row' v-for='indexX of sideLength' :key="indexX">
            <div class='col'
                 :style='{"background-color":((indexX-1)*sideLength+indexY)===theFirstFew?approximateColor:defaultColor}'
                 v-for='indexY of sideLength' :key='indexY'
                 @click='judge(((indexX-1)*sideLength+indexY)===theFirstFew?approximateColor:defaultColor)'>
                <!--<div>{{(indexX-1)*sideLength+indexY}}</div>-->
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "GameOperating",
        data() {
            return {
                sideLength: 2,
                theFirstFew: 1,
                approximateParameter: {
                    R: 0,
                    G: 0,
                    B: 0,
                    A: 0
                },
                threshold: 1,
                maxThreshold: 32,
                defaultColor: "#fff",
                approximateColor: "#000"
            }
        },
        methods: {
            /**
             * 产生随机整数，包含下限值，包括上限值
             * @param {Number} lower 下限
             * @param {Number} upper 上限
             * @return {Number} 返回在下限到上限之间的一个随机整数
             */
            randomInt(lower, upper) {
                return lower === upper ? lower : Math.floor(Math.random() * (upper - lower + 1)) + lower;
            },

            /**
             * 产生随机浮点数，包含下限值，接近上限值
             * @param {Number} lower 下限
             * @param {Number} upper 上限
             * @return {Number} 返回在下限到上限之间的一个随机整数
             */
            randomFloat(lower, upper, numberOfDigits) {
                return lower === upper ? lower : (Math.random() + this.randomInt(lower, upper - 1)).toFixed(numberOfDigits);
            },
            getRandomColor(minR, minG, minB, minA, maxR, maxG, maxB, maxA) {
                var r = this.randomInt(minR || 0, maxR || 255),
                    g = this.randomInt(minG || 0, maxG || 255),
                    b = this.randomInt(minB || 0, maxB || 255),
                    a = this.randomFloat(minA || 1, maxA || 1, 2);
                return {
                    R: r,
                    G: g,
                    B: b,
                    A: a
                }
            },
            getApproximateColor(r, g, b, a) {
                return {
                    R: r - this.approximateParameter.R,
                    G: g - this.approximateParameter.G,
                    B: b - this.approximateParameter.B,
                    A: a - this.approximateParameter.A
                }
            },
            rgbaToString(rgba) {
                return `rgba(${rgba.R}, ${rgba.G}, ${rgba.B},${rgba.A})`;
            },
            init() {
                this.defaultColor = "#fff";
                this.approximateColor = "#000"
                this.approximateParameter.R = this.maxThreshold
                this.approximateParameter.G = this.maxThreshold
                this.approximateParameter.B = this.maxThreshold
                this.theFirstFew = this.randomInt(1, this.sideLength * this.sideLength);
            },
            createColor() {
                const color = this.getRandomColor(this.approximateParameter.R, this.approximateParameter.G, this.approximateParameter.B);
                this.defaultColor = this.rgbaToString(color);
                this.approximateColor = this.rgbaToString(this.getApproximateColor(color.R, color.G, color.B, color.A))
            },
            increaseRefresh() {
                this.threshold += 1;
                this.sideLength += (this.threshold % 3 == 0 ? 1 : 0);
                this.approximateParameter.R -= 1
                this.approximateParameter.G -= 1
                this.approximateParameter.B -= 1
                this.theFirstFew = this.randomInt(1, this.sideLength * this.sideLength);
                this.createColor()
            },
            judge(color) {
                if (color == this.approximateColor) {
                    console.log('yes')
                    if (this.threshold == this.maxThreshold) {
                        console.log('结束')
                    }
                    else {
                        console.log('增加难度')
                        this.increaseRefresh()
                    }
                }
                else {
                    console.log('no')
                }
            }
        },
        mounted() {
            this.init()
            console.log('ok')

        }
    }
</script>

<style lang='scss' scoped>
    .game-operating {
        display: flex;
        width: 100%;
        height: 100%;
        flex-wrap: wrap;
        /*border: aquamarine solid 1px;*/
        .row {
            flex: 0 0 100%;
            display: flex;
            /*background-color: rgba(6, 6, 6, .5);*/
            .col {
                flex: 1;
                display: flex;
                cursor: pointer;
                /*border: aqua 1px solid;*/
                box-shadow: 0 0 1px #333;
                border-radius: 5px;
                margin: 2px;
                background-color: rgba(66, 6, 6, .5);
                justify-content: center;
                align-items: center;
            }
        }
    }
</style>
