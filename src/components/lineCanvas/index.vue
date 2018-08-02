<template>
    <canvas id="canvas">
    </canvas>
</template>
<script>
export default {
    mounted() {
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const rndCl = () => Math.floor(Math.random() * 225);
        const width = this.canvasWidth;
        const height = this.canvasHeight;
        let baseList = [];
        canvas.width = width;
        canvas.height = height;
        // 绘制园
        const drawRounds = (obj) => {
            const { x, y, r, color } = obj;
            ctx.beginPath();
            ctx.arc(x, y, r, 0, 2 * Math.PI);
            if (this.isColor) {
                ctx.fillStyle = color;
            } else {
                ctx.fillStyle = this.roundColor;
            }
            ctx.fill();
            ctx.closePath();
        };
        // 判断移动方向
        const controlDirection = (obj) => {
            if (obj.x >= (width - obj.r)) {
                obj.controlX = 'left';
            } else if (obj.x <= parseInt(obj.r / 2, 10)) {
                obj.controlX = 'right';
            }
            if (obj.y >= (height - obj.r)) {
                obj.controlY = 'bottom';
            } else if (obj.y <= parseInt(obj.r / 2, 10)) {
                obj.controlY = 'top';
            }
            return obj;
        };
        // 划线
        const drawLine = (list) => {
            list.forEach((item) => {
                ctx.beginPath();
                ctx.moveTo(item.x1, item.y1);
                ctx.lineTo(item.x2, item.y2);
                ctx.LineWeight = 1;
                if (this.isColor) {
                    ctx.strokeStyle = item.color;
                } else {
                    ctx.strokeStyle = this.lineColor;
                }
                ctx.stroke();
                ctx.closePath();
            });
        };
        // 绘制
        const draw = (list) => {
            const dotsArr = [];
            const lineArr = [];
            list.forEach((item) => {
                const xy = controlDirection(item);
                const obj = roundMove(xy);
                dotsArr.push(obj);
            });
            dotsArr.forEach((item1) => {
                dotsArr.forEach((item2) => {
                    if (item1 !== item2) {
                        const x = item1.x - item2.x;
                        const y = item1.y - item2.y;
                        if (Math.abs(x) < 150 && Math.abs(y) < 150) {
                            const obj = {
                                x1: item1.x,
                                y1: item1.y,
                                x2: item2.x,
                                y2: item2.y,
                                color: item1.color,
                            };
                            lineArr.push(obj);
                        }
                    }
                });
            });
            drawLine(lineArr);
            dotsArr.forEach((item, index) => {
                drawRounds(item, index);
            });
            baseList = dotsArr;
            setTimeout(() => {
                // if(this.paused){
                reDraw();
                // }
            }, 50);
        };
        const reDraw = () => {
            ctx.clearRect(0, 0, width, height);
            draw(baseList);
        };
        // 移动
        const roundMove = (obj) => {
            switch (obj.controlX) {
            case 'right':
                obj.x++;
                break;
            case 'left':
                obj.x--;
                break;
            default:
            }
            switch (obj.controlY) {
            case 'top':
                obj.y++;
                break;
            case 'bottom':
                obj.y--;
                break;
            default:
            }
            return obj;
        };
        // 创造圆点
        const creatDots = () => {
            const arr = [];
            for (let i = 0; i < this.dotsNum; i++) {
                const obj = {};
                obj.x = parseInt(Math.random() * width, 10);
                obj.y = parseInt(Math.random() * height, 10);
                obj.r = parseInt(Math.random() * 10, 10);
                obj.controlX = parseInt(Math.random() * 10, 10) > 5 ? 'left' : 'right';
                obj.controlY = parseInt(Math.random() * 10, 10) > 5 ? 'bottom' : 'top';
                obj.color = `rgba(${rndCl()},${rndCl()},${rndCl()},.3)`;
                arr.push(obj);
            }
            return arr;
        };
        draw(creatDots());
        // 鼠标移动
        const moveXY = (event) => {
            const obj = {};
            obj.x = event.clientX;
            obj.y = event.clientY;
            obj.r = 0;
            baseList[0] = obj;
            baseList[0].r = 1;
        };
        // 鼠标点击
        const addXY = (event) => {
            const obj = {};
            obj.x = event.clientX;
            obj.y = event.clientY;
            obj.r = parseInt(Math.random() * 10, 10);
            obj.color = `rgba(${rndCl()},${rndCl()},${rndCl()},.3)`;
            obj.controlX = parseInt(Math.random() * 10, 10) > 5 ? 'left' : 'right';
            obj.controlY = parseInt(Math.random() * 10, 10) > 5 ? 'bottom' : 'top';
            baseList.push(obj);
        };
        window.addEventListener('mousemove', moveXY);
        window.addEventListener('click', addXY);
    },
    data() {
        return {
        };
    },
    props: {
        dotsNum: {
            type: Number,
            default: 50,
        },
        isColor: {
            type: Boolean,
            default: true,
        },
        roundColor: {
            type: String,
            default: '#999',
        },
        lineColor: {
            type: String,
            default: '#ccc',
        },
        canvasWidth: {
            type: Number,
            default: 300,
        },
        canvasHeight: {
            type: Number,
            default: 200,
        },
    },
};
</script>
