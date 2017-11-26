<template>
  <div class="trainig">
     <h1>Math training. Level {{ level + 1 }}</h1>
     <hr>
    <div class="progress">
      <div class="progress-bar" :style="progresStyles"></div>
    </div>
     <div class="box">
       <transition name="flip" mode="out-in">


    <app-start-screen
      v-if="state == 'start'"
      @onStart = "onStart"
    >

    </app-start-screen>
    <app-question v-else-if=" state == 'question'"
      @success="onQuestSuccess"
      @error="onQuestError"
      :settings="levels[level]"
    ></app-question>
    <app-result-screen v-else-if="state == 'result'"
      :stats="stats"
      @repeat="onStart"
      @nextLevel="onNextLevel"
    ></app-result-screen>
    <app-message v-else-if=" state == 'message'"
      :type="message.type"
      :text="message.text"
      @onNext="onNext"
    ></app-message>
    <div v-else>Unknown state</div>
     </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
     state: 'start',
     stats: {
       success: 0,
       error: 0
     },
     message: {
       type: '',
       text: ''
     },
     questMax: 3,
     level: 0,
     levels: [
       {
         from: 10,
         to: 20,
         range: 5,
         variants: 4
       },
       {
         from: 50,
         to: 100,
         range: 10,
         variants: 4
       },
        {
         from: 500,
         to: 1000,
         range: 40,
         variants: 6
       }
     ]
    }
  },
  methods: {
    onStart(){
      this.state='question';
      this.stats.success = 0;
      this.stats.error = 0;
    },
    onQuestSuccess(){
      this.state='message';
      this.message.text = 'Good Job';
      this.message.type = 'success';
      this.stats.success++
    },
    onQuestError(msg){
      this.state='message';
      this.message.text = msg;
      this.message.type = 'warning';
      this.stats.error++
    },
    onNext(){
      if (this.questDone < this.questMax) {
          this.state='question';
      } else {
        this.state='result';
      }
    },
    onNextLevel() {
      console.log('------------------------------------');
      console.log(this.level, this.levels.length);
      console.log('------------------------------------');
            if (this.level+1 == this.levels.length) {
              this.state='start';
              this.level=0;
            } else {
              this.level++
              this.onStart();
            }
      }
  },
  computed: {
    questDone() {
      return this.stats.success + this.stats.error;
    },
    progresStyles() {
      return {
        width: (this.questDone / this.questMax * 100) + "%"
        }
    }
  }
}
</script>

<style scoped>
  .progress {
    margin: 20px auto;
  }
  .progress-bar {
    -webkit-transition: width 0.5s;
    -moz-transition: width 0.5s;
    transition: width 0.5s;
  }
  .trainig {
    max-width: 700px;
    margin: 20px auto;
  }
  .flip-enter {

  }
  .flip-enter-active {
    animation: flipInX 0.3s linear;
  }
   .flip-leave {

  }
  .flip-leave-active {
    animation: flipOutX 0.3s linear;
  }
  @keyframes flipInX{
    from{transform: rotateX(90deg);}
    to{transform: rotateX(0deg);}

  }
  @keyframes flipOutX{
    from{transform: rotateX(0deg);}
    to{transform: rotateX(90deg);}

  }
</style>
