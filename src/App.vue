<template>
  <div id="app">

    <h1>Vuemon</h1>

    <div id="simon">

      <div id="status">
      </div>

      <div class="row">
        <div id="green" class="light col" v-bind:class="{'bright':greenBright}" v-on:click="captureTap('green')"></div>
        <div id="red" class="light col" v-bind:class="{'bright':redBright}" v-on:click="captureTap('red')"></div>
      </div>

      <div class="row">
        <div id="yellow" class="light col" v-bind:class="{'bright':yellowBright}" v-on:click="captureTap('yellow')"></div>
        <div id="blue" class="light col" v-bind:class="{'bright':blueBright}" v-on:click="captureTap('blue')"></div>
      </div>

    </div>

    <div id="controls" class="row">
      <div class="col"><button class="start" v-on:click="start">Start</button></div>
    </div>

    <div id="history">
      <p><strong>Current Sequence:</strong> {{ current }}</p>
      <p><strong>Longest Sequence:</strong> {{ longest }}</p>
    </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      longest: 0,
      sequence: [],
      taps: [],
      greenBright: false,
      redBright: false,
      yellowBright: false,
      blueBright: false,
      sequenceCountdown: 0,
      canTap: false,
      lights: [ 'red', 'green', 'yellow', 'blue' ]
    }
  },
  computed: {
    current: function() {
      return this.sequence.length;
    }
  },
  mounted() {

  },
  methods: {

    start: function() {
      this.sequence = [];
      this.addToSequence();
      this.playSequence();
      this.startTimer();
    },

    chooseRandomLight: function() {
      var index = Math.floor(Math.random() * 4);
      console.log(index);
      return this.lights[index];
    },

    addToSequence: function() {
      this.sequence.push(this.chooseRandomLight());
    },

    playSequence: function() {
      
      this.sequenceCountdown=this.sequence.length;

      for(var i=0; i<this.sequence.length; i++){
        this.makeBright();
        }
    },

    makeBright: function(){
      if(this.sequence[0]==='green'){
        this.greenBright=true;
      }
      else if(this.sequence[0]==='yellow'){
        this.yellowBright=true;
      }
      else if(this.sequence[0]==='red'){
        this.redBright=true;
      }
      else{
        this.blueBright=true;
      }
           
      // this calls the reset function
      setTimeout(this.makeDim, 1000);
      
    },

    makeDim: function(){
      console.log(this.sequence[0] + " back to dim");
      // this needs to set the color BACK TO NORMAL
      // this.sequence[0] is how to capture the current color
      if(this.greenBright){
          this.greenBright = !this.greenBright;
      }
      else if(this.yellowBright){
          this.yellowBright = !this.yellowBright;
      }
      else if(this.redBright){
        this.redBright = !this.redBright;
      }
      else{
        this.blueBright = !this.blueBright;
      }

      // if there is more left to cycle through
          if(this.sequenceCountdown>0){
            console.log("the sequence countown is: " + this.sequenceCountdown);
            console.log('this is where you make the squares clickable');
            this.canTap=true;
            
            
            this.sequenceCountdown--;
            console.log("sequence countdown is: " + this.sequenceCountdown);
           //I want this to start it over again
          }

      // console.log(this.sequence[]);

    },

    captureTap: function(color) {
        if(this.canTap){
          alert('you tapped - ' + color);
        }
        if(this.sequenceCountdown>0){
          this.playSequence();
        }
        else{
          alert('you done');
        }
    },

    startTimer: function() {

    }

  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#simon {
  padding: 20px;
}

#controls {
  padding-bottom: 20px;
}

#status {
  padding-bottom: 20px;
}

.row {

}

.col {
  display: inline-block;
}

.start {
  padding: 10px;
  font-size: 18px;
}

.light {
  margin: 20px;
  border: 1px solid #000;
}

#red {
  height: 100px;
  width: 100px;
  background: #E53A40;
  opacity: 0.1;
}

#yellow {
  height: 100px;
  width: 100px;
  background: #EFDC05;
  opacity: 0.1;
}

#green {
  height: 100px;
  width: 100px;
  background: #75D701;
  opacity: 0.1;
}

#blue {
  height: 100px;
  width: 100px;
  background: #30A9DE;
  opacity: 0.1;
}

.bright {
  opacity: 1.0 !important;
}



</style>
