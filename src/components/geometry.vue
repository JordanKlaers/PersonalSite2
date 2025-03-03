<template>
	<div id="geometry-container">
        <span class="dark visible shape square-1-a"></span>
        <span class="light shape square-1-a"></span>
        <span class="dark visible shape square-1-b"></span>
        <span class="light shape square-1-b"></span>

        <span class="dark visible shape square-2-a"></span>
        <span class="light shape square-2-a"></span>
        <span class="dark visible shape square-2-b"></span>
        <span class="light shape square-2-b"></span>
        <span class="dark visible shape square-2-c"></span>
        <span class="light shape square-2-c"></span>
        <span class="dark visible shape square-2-d"></span>
        <span class="light shape square-2-d"></span>
        <span class="shape square-3-a"></span>
        <span class="shape square-3-b"></span>
        <span class="shape square-3-c"></span>
        <template v-if="!isInternetExplorer">
            <span
                v-for="curve in curveElements"
                :key="curve.class"
                class="shape"
                :id="curve.id"
                :style="setClipPath(curve.degree, curve.startDegree)"
                >
            </span>
        </template>
	</div>
</template>

<script>
export default {
	name: 'geometryContainer',
    data() {
        return {
            path: '',
            curveElements: [
                { id: 'curve-1-a', degree: 80, startDegree: 0 },
                { id: 'curve-1-b', degree: 80, startDegree: 45 },
                { id: 'curve-1-c', degree: 80, startDegree: 75 },
                { id: 'curve-2-a', degree: 130, startDegree: 20 },
                { id: 'curve-2-b', degree: 130, startDegree: 20 }
            ]
        }
    },
    mounted() {

    },
    methods: {
        setClipPath(degree, startDegree) {
            //start at center
            let path = '50% 50%';
            //add first point that matches the starting degree (0 = bottom, 90 = right, 180 = top, 270 = left, 360 = bottom)
            path += `,${Math.floor(this.x(startDegree, 50))+50}% ${Math.floor(this.y(startDegree, 50))+50}%`;
            //add the corner points between the start and end degree, this prevents the arc from being cut off if it exceeds 180 degrees
            let  closurePoints = [', 100% 100%', ', 100% 0%',  ', 0% 0%', ', 0% 100%'];
            path += closurePoints.slice(Math.floor(startDegree / 90), Math.floor((degree + startDegree) / 90) + 1).join('');
            //add the final point
            path += `,${Math.floor(this.x(degree + startDegree, 50))+50}% ${Math.floor(this.y(degree + startDegree, 50))+50}%`;
            return `clip-path: polygon(${path})`;
        },
        setClipPathSVG(degree, startDegree, asClipPathOnly) {
            // return '.5 .5, 0.5 1, 1 1, 1 .5, .5 .5';
            
            //start at center
            let path = '.5 .5';
            //add first point that matches the starting degree (0 = bottom, 90 = right, 180 = top, 270 = left, 360 = bottom)
            path += `,${this.x(startDegree, .5) +.5} ${this.y(startDegree, .5)+.5}`;
            //add the corner points between the start and end degree, this prevents the arc from being cut off if it exceeds 180 degrees
            let  closurePoints = [', 1 1', ', 1 0',  ', 0 0', ', 0 1'];
            path += closurePoints.slice(Math.floor(startDegree / 90), Math.floor((degree + startDegree) / 90) + 1).join('');
            //add the final point
            path += `,${(this.x(degree + startDegree, .5))+.5} ${(this.y(degree + startDegree, .5))+.5}, 0.5 0.5`;
            return path.replace(/\%/g, '');
        },
        x: (degree, radius) => {
           return (Math.sin(degree*(Math.PI/180))*radius);
        },
        y: (degree, radius) => {
            return (Math.cos(degree*(Math.PI/180))*radius);
        }
    }
};
</script>
<style lang='scss' type="text/scss">
@import '~_scss_/_mixins';

#geometry-container {
    position: fixed;
    z-index: 2;
    height: 75vh;
    width: 0px;
    right: 0;
    top: 0;
    .shape {
        position: absolute;
        @include square-diagonal-split((
            className: "square-1-a",
            height: 160,
            width: 160,
            top: -35,
            right: -20,
            dark-before-one: var(--secondary-1-a),    dark-before-two: var(--secondary-1-b),
            dark-after-one: var(--secondary-1-c),       dark-after-two: var(--secondary-1-d),
            light-before-one: var(--secondary-1-c),    light-before-two: var(--secondary-1-d),
            light-after-one: var(--secondary-1-a),       light-after-two: var(--secondary-1-b),
            light-after-gradient-dir: to bottom
        ));
        @include square-diagonal-split((
            className: "square-1-b",
            height: 80,
            width: 80,
            top: 180,
            right: 15,
            rotate: 45deg,
            dark-before-one: var(--secondary-1-a),    dark-before-two: var(--secondary-1-b),
            dark-after-one: var(--secondary-1-c),       dark-after-two: var(--secondary-1-d),
            light-before-one: var(--secondary-1-c),    light-before-two: var(--secondary-1-d),
            light-after-one: var(--secondary-1-a),       light-after-two: var(--secondary-1-b),
            light-after-gradient-dir: to bottom,
            z-index: 2
        ));
        &.square-1-b {
            --rotate: 0deg;
            transform: perspective(400px) rotateY(var(--rotate)) rotateZ(45deg);
        }
        @include square-diagonal-split((
            className: "square-2-a",
            height: 130,
            width: 130,
            top: 68,
            right: 107,
            rotate: -135deg,
            dark-before-one: var(--primary-1-a),    dark-before-two: var(--primary-1-b),
            dark-after-one: var(--primary-1-c),       dark-after-two: var(--primary-1-d),
            light-before-one: var(--primary-1-a),       light-before-two: var(--primary-1-b),
            light-after-one: var(--primary-1-c),        light-after-two: var(--primary-1-d)
        ));
        @include square-diagonal-split((
            className: "square-2-b",
            height: 110,
            width: 110,
            top: 230,
            right: 117,
            rotate: 135deg,
            dark-before-one: var(--primary-1-a),    dark-before-two: var(--primary-1-b),
            dark-after-one: var(--primary-1-c),       dark-after-two: var(--primary-1-d),
            light-before-one: var(--primary-1-a),       light-before-two: var(--primary-1-b),
            light-after-one: var(--primary-1-c),        light-after-two: var(--primary-1-d),
            z-index: 2
        ));
        @include square-diagonal-split((
            className: "square-2-c",
            height: 60,
            width: 60,
            top: 270,
            right: 260,
            rotate: -25deg,
            dark-before-one: var(--primary-1-a),    dark-before-two: var(--primary-1-b),
            dark-after-one: var(--primary-1-c),       dark-after-two: var(--primary-1-d),
            light-before-one: var(--primary-1-a),       light-before-two: var(--primary-1-b),
            light-after-one: var(--primary-1-c),        light-after-two: var(--primary-1-d)
        ));
        &.square-2-c {
            --rotateX: 0deg;
            --translateY: 0px;
            transform: rotateZ(-25deg) perspective(400px) rotateX(var(--rotateX)) translateY(var(--translateY));
        }
        @include square-diagonal-split((
            className: "square-2-d",
            height: 20,
            width: 20,
            top: 25,
            right: 255,
            rotate: -50deg,
            dark-before-one: var(--primary-1-a),    dark-before-two: var(--primary-1-b),
            dark-after-one: var(--primary-1-c),       dark-after-two: var(--primary-1-d),
            light-before-one: var(--primary-1-a),       light-before-two: var(--primary-1-b),
            light-after-one: var(--primary-1-c),        light-after-two: var(--primary-1-d)
        ));
        &.square-2-d {
            --right: 255px;
            right: var(--right);
        }
        &.square-3-a {
            transition: background-color 0.7s linear, box-shadow 0.7s linear, transition 0.28s linear, top 0.28s linear;
            position: absolute;
            z-index: 2;
            height: 160px;
            width: 190px;
            top: 120px;
            right: 172px;
            background-color: var(--neutral-2);
            box-shadow: var(--neutral-2-shadow);
            opacity: 1;
            transform: perspective(400px) rotateY(159deg) skewX(-7deg);
        }
        &.square-3-b {
            transition: all 0.7s linear;
            z-index: 3;
            height: 90px;
            width: 90px;
            top: 90px;
            right: 115px;
            background-color: var(--neutral-2);
            box-shadow: var(--neutral-2-shadow);
            opacity: 1;
            transform: rotateZ(25deg);
        }
        &.square-3-c {
            transition: all 0.7s linear;
            z-index: 6;
            height: 35px;
            width: 35px;
            top: 180px;
            right: 370px;
            background-color: var(--neutral-2);
            box-shadow: var(--neutral-2-shadow);
            opacity: 1;
            transform: rotateZ(25deg);
        }
        @include curve((
            id: "curve-1-a",
            top: 80px,
            right: 330px,
            width: 150px,
            height: 100px,
            border-radius: 50%,
            border: 2px solid var(--primary-line-1),
            transform: rotate(28deg),
            z-index: 1
        ));
        @include curve((
            id: "curve-1-b",
            top: -80px,
            right: 225px,
            width: 80px,
            height: 300px,
            border-radius: 50%,
            border: 2px solid var(--primary-line-1),
            transform: rotate(-215deg),
            z-index: 1
        ));
        @include curve((
            id: "curve-1-c",
            top: 215px,
            right: 240px,
            width: 140px,
            height: 140px,
            border-radius: 50%,
            border: 2px solid var(--primary-line-1),
            transform: rotate(180deg),
            z-index: 1
        ));
        &#curve-1-c {
            --rotate: 180deg;
            transform: rotate(var(--rotate));
        }
        @include curve((
            id: "curve-2-a",
            top: 130px,
            right: 20px,
            width: 60px,
            height: 90px,
            border-radius: 50%,
            border: 2px solid var(--secondary-line-1),
            transform: rotate(110deg),
            z-index: 1
        ));
        @include curve((
            id: "curve-2-b",
            top: 270px,
            right: 160px,
            width: 30px,
            height: 130px,
            border-radius: 50%,
            border: 2px solid var(--secondary-line-1),
            transform: rotate(-30deg),
            z-index: 1
        ));
    }
}
</style>


