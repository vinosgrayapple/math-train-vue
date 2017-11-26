<template>
<div class="alert">
<h3>
  {{ x }} + {{ y }} = ?
</h3>
<hr>
<div class="buttons" >
  <button class="btn btn-success"
    v-for="number in answer"
    @click="onAnswer(number)"
  >
    {{number}}
  </button>
</div>
</div>
</template>

<script>
export default {
  props: ['settings'],
  data(){

    return {
      x: mtRnd(this.settings.from,this.settings.to),
      y: mtRnd(this.settings.from,this.settings.to)
    }
  },
  computed: {
    good(){
      return this.x + this.y
    },
    answer(){
      let res = [this.good];
      while(res.length < this.settings.variants) {
        const num = mtRnd(this.good - this.settings.range, this.good + this.settings.range);
        if (res.indexOf(num) === -1){

          res.push(num);
        }
      }
      return res.sort(() => Math.random() > 0.5);
    }
  },
  methods: {
    onAnswer(num){
      if (num == this.good) {
        this.$emit('success');
      } else {
         this.$emit('error', `${this.x} + ${this.y} = ${this.good}`);
      }
    }
  }

}
function mtRnd(min,max){
   let  diff = max - min;
  return Math.floor(Math.random() * (diff + 1)) + min;
 }
</script>

<style scoped>
  .alert {
    padding-top: 20px;
    background-color: #eee;
    text-align: center;
  }
  .buttons {
    display: flex;
    justify-content: space-around;
  }
  .btn {
    margin: 20px auto;
  }
</style>
