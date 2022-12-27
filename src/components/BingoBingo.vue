<template>
    <div id="bg">
        <div id="logobackground" class="ball" v-show="disabled"></div>

        <div class="row">

            <div id="cell_1">

                <div id="banner"></div>
                <div id="board">
                    <transition-group tag="ul" class="number-list" name="list">
                        <li v-for="(item, index) in list" :key="index"
                            v-bind:class="{ 'item': pool.indexOf(item) < 0, 'ball': pool.indexOf(item) < 0, 'roll': pool.indexOf(item) >= 0 }"
                        >{{ item }}</li>
                    </transition-group>
                </div>
            </div>

            <div id="cell_2">
                <!-- <div id="button" @click="!disabled && draw()"></div> -->
                <!-- <button @click="list.length = 0">Remove all</button> -->
            </div>

        </div>
        
        <div id="logo"></div>
        <div id="button" @click="!disabled && draw()"></div>
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
            this.disabled = false;
        }
    }


}

</script>

<style lang="scss">
#bg {
    width: 100vw;
    height: 100vh;
    // min-width: 1210px;
    // min-height: 950px;
    //background: rgba(32, 32, 32, 0.205);
    background: url("~/public/background_clear.jpg") no-repeat;
    background-size: 100% 100%;//contain;
    position: relative; 

}
#logo{
    width: 20vw;
    height: 20vw; 
    background: url("~/public/logo.png") no-repeat;
    background-size: 100% 100%;//contain;
    position: fixed;
    right: 2vw;
    top: 40vh;
}
#logobackground{
    width: 20vw;
    height: 20vw; 
    position: fixed;
    right: 2vw;
    top: 40vh;

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
    width: 24vw;
    //height: 100%;
    //background: #454c4474;
    padding-top: 83vh;
    padding-right: 1vw;

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


#button {
    width: 6vw;
    height: 6vw;
    cursor: pointer;
    margin: 0 auto;
    // background-color: #e82a2a;  
    // color:#ffffff;
    // text-align: center;
    // line-height: 6vw;
    // font-size: 5vw;
    // font-weight:bold;
    background: url("~/public/button.png") no-repeat;
    background-size: 100% 100%;//contain;
    position: fixed;
    right: 8.5vw;
    top: 83vh;
}

.roll {
    width: 17vw;
    height: 17vw;
    text-align: center;
    line-height: 17vw;
    font-size: 13vw;
    position: fixed;
    right: 1vw;
    top: 42vh;
    color: rgb(255, 255, 255);
}

.ball {
    background-color: #e82a2a;
    color: rgb(255, 255, 255);
    border-radius: 99em;
}

.number-list {
    //overflow: hidden;
    padding: 0;
    margin-bottom: 1rem;
    list-style-type: none;
    margin: 0;

    .item {
        display: block;
        float: left;
        width: 8vw;
        height: 8vw;
        text-align: center;
        line-height: 8vw;
        font-size: 6vw;
        margin-right: 1rem;
        margin-left: 0;
        margin-bottom: 1rem;
    }
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