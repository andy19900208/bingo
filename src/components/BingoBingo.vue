<template>
    <div id="bg">
        <div class="row">

            <div id="cell_1">
                <transition-group tag="ul" class="number-list" name="list">
                    <li v-for="(item, index) in list" :key="index" class="ball "
                        v-bind:class="{ 'item': pool.indexOf(item) < 0, 'roll': pool.indexOf(item) >= 0 }">{{ item }}</li>
                </transition-group>
            </div>

            <div id="cell_2">
                <div id="button" @click="!disabled && draw()">抽</div>
                <!-- <button @click="list.length = 0">Remove all</button> -->
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
    //background: url("~/public/image/background/聖誕抽獎版面.jpg") no-repeat;
    background-size: 100% 100%;//contain;
    position: relative; 

}
.row {
    display: flex;
    height: 100vh;
}
#cell_1 {
        width: 75vw;
        //height: 100%;
        background: #19015c2d;
        padding-top: 3vw;
        padding-left: 3vw;
}
#cell_2 {
    width: 25vw;
    //height: 100%;
    background: #454c4474;
    padding-top: 55vh;

}

#button {
    width: 18vw;
    height: 18vw;
    cursor: pointer;
    margin: 0 auto;
    background-color: #c33e3e71;  
}

.roll {
    width: 20vw;
    height: 20vw;
    text-align: center;
    line-height: 20vw;
    font-size: 17vw;
    position: fixed;
    right: 2vw;
    top: 5vh;
}

.ball {
    background-color: rgb(255, 238, 0);
    color: rgb(0, 0, 0);
    border-radius: 99em;
}

.number-list {
    overflow: hidden;
    padding: 0;
    margin-bottom: 1.5rem;
    list-style-type: none;

    .item {
        display: block;
        float: left;
        width: 10vw;
        height: 10vw;
        text-align: center;
        line-height: 10vw;
        font-size: 8vw;
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