<template>
  <div>
    <button @click="draw">抽</button>
    <button @click="list.length = 0">Remove all</button>

    <transition-group tag="ul" class="number-list" name="list">
      <li v-for="(item, index) in list" 
          :key="index" 
          class="ball "
          v-bind:class="{'item': pool.indexOf(item) < 0 || true, }"
      >{{ item }}</li>
    </transition-group>
    
  </div>
</template>

<script>
export default {
  name: 'BingoBingo',

  data() {
    return {
      times: 0,
      pool: [],
      list: []
    }
  },

  mounted() {
    for (let i = 1; i <= 10; i++)
    {
      this.pool.push(i);  
    }
      
  },

    watch:
    {
        times() {
            let n = Math.floor(Math.random() * this.pool.length);
            this.list[ this.list.length - 1] = this.pool[n];
            //console.log(this.pool[n]);

            if (this.times > 0) {
                let timer = 1;

                if(this.times > 20) timer = 100;
                else if(this.times > 10) timer = 150;
                else if(this.times > 5) timer = 200;
                else if(this.times == 5) timer = 250;
                else if(this.times == 4) timer = 300;
                else if(this.times == 3) timer = 350;
                else if(this.times == 2) timer = 400;
                else if(this.times == 1) timer = 500;

                setTimeout(() => {
                    this.times--;
                }, timer);
                
            }
            else {
                this.add(n);
            }

        }
    },


  methods: {
    
    draw(){
      if (this.pool.length == 0) {
        alert('empty');
        return;
      }
      this.times = 50,
      this.list.push('');

      //let n = Math.floor(Math.random() * this.pool.length);

      

    },

    add(index) {
        

        this.pool.splice(index, 1);
    }
  }


}

</script>

<style lang="scss">
  button {
    font-size: 1rem;
    margin-bottom: 1rem;
    margin-right: 1rem;
  }

  #main_ball{
    width: 250px;
    height: 250px;
    text-align: center;
    line-height: 250px;
    font-size:200px;
  }

  .ball{
    background-color: rgb(255, 238, 0);
    color: rgb(0, 0, 0);
    border-radius: 99em;
  }

  .number-list {
    overflow: hidden;
    padding: 0;
    margin-bottom: 1.5rem;

    .item {
      display: block;
      float: left;
      width: 150px;
      height: 150px;
      text-align: center;
      line-height: 150px;
      font-size:100px;
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