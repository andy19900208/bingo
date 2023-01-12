<template>
    <div id="bg">
        <div class="row">

            <div id="cell_1">

                <div id="banner"></div>
                <div id="board">
                    <transition-group tag="ul" name="list"
                                      class="number-list"
                                      :class="{ overflow: list.length > 32 }"
                    >
                        <li v-for="(item, index) in list" :key="index" class="ball "
                            v-bind:class="{ 'item': pool.indexOf(item) < 0, 'roll': pool.indexOf(item) >= 0 }"
                        >{{ item }}</li>
                    </transition-group>
                </div>
            </div>

            <div id="cell_2">
                <div id="button" class="button" :class="{'flash': disabled}"
                     @click="!disabled && draw()"
                ></div>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    name: 'BingoBingo',

    data() {
        return {
            disabled: false,
            times: 0,
            pool: [],
            list: []
        }
    },

    mounted() {
        for (let i = 1; i <= 75; i++) {
            this.pool.push(i);
        }

        document.addEventListener( "keydown", this.onKeydown );
    },

    watch:
    {
        times() {
            let n = Math.floor(Math.random() * this.pool.length);
            this.list[this.list.length - 1] = this.pool[n];
            //console.log(this.pool[n]);

            if (this.times > 0) {
                let timer = 1;

                if (this.times > 20) timer = 50;
                else if (this.times > 10) timer = 80;
                else if (this.times > 5) timer = 100;
                else if (this.times > 2) timer = 120;
                else if (this.times == 2) timer = 160;
                else if (this.times == 1) timer = 200;

                setTimeout(() => {
                    this.times--;
                }, timer);

            }
            else {
                setTimeout(() => {
                    this.add(n);
                }, 500);
                
            }

        }
    },

    methods: {

        draw() {
            if (this.pool.length == 0) {
                alert('empty');
                return;
            }
            this.disabled = true;
            this.times = 30,
            this.list.push('');

            //let n = Math.floor(Math.random() * this.pool.length);
        },

        add(index) {
            this.pool.splice(index, 1);
            
            setTimeout(() => {
                this.disabled = false;
            }, 750);
        },

        onKeydown( event ) {
            if(event.key === " " && !this.disabled ){       
                this.draw();
            }
        },
        
    }

}

</script>

<style lang="scss">
@keyframes Bggif {
    0%    { background-image: url('~/public/background_01.png');}
    25%   { background-image: url('~/public/background_02.png');}
    50%   { background-image: url('~/public/background_01.png');}
    75%   { background-image: url('~/public/background_02.png');}
    100%  { background-image: url('~/public/background_01.png');}
}

#bg {
    width: 100vw;
    height: 100vh;
    // min-width: 1210px;
    // min-height: 950px;
    //background: rgba(32, 32, 32, 0.205);
    background: url("~/public/background.png") no-repeat;
    background-size: 100% 100%;//contain;
    position: relative; 

    animation-name: Bggif;
    animation-duration: 1s;
    animation-delay: 0s;
    animation-iteration-count: infinite;

}
.row {
    display: flex;
    height: 100vh;
}
#cell_1 {
        width: 75vw;
        //height: 100%;
        //background: #19015c2d;
        //padding-top: 3vw;
        //padding-left: 3vw;
}
#cell_2 {
    width: 25vw;
    //height: 100%;
    //background: #454c4474;
    padding-top: 55vh;

}
#banner{
    height: 25vh;
    //background: #4c4c4474;
}
#board
{
    padding: 5vh 2vw 0 2vw;
    //height: 80vh;
}

@keyframes ButtonFlash {
    0%    { background-image: url('~/public/button@2x_default.png');}
    25%   { background-image: url('~/public/button@2x_hover.png');}
    50%   { background-image: url('~/public/button@2x_default.png');}
    75%   { background-image: url('~/public/button@2x_hover.png');}
    100%  { background-image: url('~/public/button@2x_default.png');}
}
.button {
    width: 18vw;
    height: 18vw;
    cursor: pointer;
    //margin: 0 auto;
    //background-color: #c33e3e71;  
    background: url('~/public/button@2x_default.png') no-repeat center center transparent;
    background-size: 60% 60%;
    margin: -28px auto 0;
    
    &.flash{
        background-image: url('~/public/button@2x_hover.png');
        animation-name: ButtonFlash;
        animation-duration: 1s;
        animation-delay: 0s;
        animation-iteration-count: 3;
    }
    &:hover {
        background-image: url('~/public/button@2x_hover.png');
    }
}

.roll {
    width: 20vw;
    height: 20vw;
    text-align: center;
    line-height: 20vw;
    font-size: 11vw;
    font-weight: bold;
    position: fixed;
    right: 2vw;
    top: 5vh;
}

.ball {
    background: url('~/public/ball_02.png') no-repeat center center scroll transparent;
    background-size: contain;
    color: #341f01;
    border-radius: 99em;
}

.number-list {
    //overflow: hidden;
    padding: 0;
    //margin-bottom: 1rem;
    list-style-type: none;
    margin: 0;
    height: 63.5vh;
    // overflow-y: auto;
    // overflow-x: hidden;

    .item {
        display: block;
        float: left;
        width: 8vw;
        height: 8vw;
        text-align: center;
        line-height: 8vw;
        font-size: 5vw;
        font-weight: bold;
        margin-right: 0.3rem;
        margin-left: 0.3rem;
        margin-bottom: 1rem;

        &:nth-child(8n+1) {
            margin-left: 0.7rem;
        }
        &:nth-child(8n) {
            margin-right: 0;
        }
    }
}
.overflow {
    overflow-y: auto;
    overflow-x: hidden;   
}

.list-enter-active,
.list-leave-active,
.list-move {
    transition: opacity 0.7s, transform 0.7s;
}

.list-leave-active {
    position: absolute;
}

.list-enter-from {
    opacity: 0;
    transform: translateY(-20px);
}

.list-leave-to {
    opacity: 0;
    transform: translateY(20px);
}
</style>