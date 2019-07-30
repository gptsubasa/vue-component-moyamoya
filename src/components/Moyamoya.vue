<template>
<div class="moyamoya">
    <div class="outer">
        <img src="../assets/realtime_ttl_deco.png" alt="MOYAMOYA その弐"  width="50" height="50"/>
    </div>
    <canvas :width="width" :height="height"></canvas>
</div>
</template>

<script>
export default {
    name: 'Moyamoya',
    props: {
        width: Number,
        height: Number,
        max: Number,
        color: String
    },
    data () {
        return {
            canvas: null,
            ctx: null,
            points: [],
            radius: this.width/2 * 0.9,
            fps: 20,
            center: {
                x: this.width/2,
                y: this.height/2
            }
        }
    },
    methods: {
        init () {
            this.canvas = this.$el.getElementsByTagName("canvas")[0];
            this.ctx = this.canvas.getContext("2d");
            this.timer = setInterval(this.update, 1000 / this.fps);

            this.$el.setAttribute("style", "width:"+this.width+"px;height:"+this.height+"px;");
        },
        update () {
            for (var i = 0; i < this.max; i++) {
                this.points[i] = this.updatePoint(this.points[i]);
            }
            this.draw();
        },
        point (c, r, rota) {
            this.x, this.y;
            this.centerX = c.x;
            this.centerY = c.y;
            this.radian = rota * (Math.PI / 180);
            this.radius = r;

            this.speed = Math.random() * 10 + 5;
            this.r = Math.random() * 0.05 + 0.5;
            this.rota = 0;
        },
        updatePoint (_point) {
            var that = _point,
                plus = Math.cos(that.rota * (Math.PI / 180)) * that.r;

			that.radius += plus;

			var cos = Math.cos(that.radian) * that.radius;
			var sin = Math.sin(that.radian) * that.radius;

			that.x = cos + that.centerX;
			that.y = sin + that.centerY;

			that.rota += that.speed;

			if (that.rota > 360) {
				that.rota -= 360;
            }
            return that;
        },
        draw () {
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

            this.ctx.strokeStyle = "#fff";
            this.ctx.stroke();
            this.ctx.lineWidth = 5;

            this.ctx.fillStyle = this.color;
            this.ctx.fill();

            this.ctx.beginPath();

            var xc1 = (this.points[0].x + this.points[this.max - 1].x) / 2;
            var yc1 = (this.points[0].y + this.points[this.max - 1].y) / 2;

            this.ctx.moveTo(xc1, yc1);

            for (var i = 0; i < this.max - 1; i++) {
                var xc = (this.points[i].x + this.points[i + 1].x) / 2;
                var yc = (this.points[i].y + this.points[i + 1].y) / 2;
                this.ctx.quadraticCurveTo(this.points[i].x, this.points[i].y, xc, yc);
            }

            this.ctx.quadraticCurveTo(this.points[i].x, this.points[i].y, xc1, yc1);
            this.ctx.closePath();
        }
    },
    mounted: function () {
        this.init();
    },
    created () {
        var rota = 360 / this.max, i;
		for (i = 0; i < this.max; i++) {
			this.points[i] = new this.point(this.center, this.radius, rota * i);
        }
    }
}
</script>

<style>
.moyamoya {
    position: relative;
    float: left;
}
.moyamoya .outer {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
    -webkit-transform: translateY(-50%) translateX(-50%);
}
</style>