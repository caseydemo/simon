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
     <!--  <p><strong>Current Sequence:</strong> {{ current }}</p>
      <p><strong>Longest Sequence:</strong> {{ longest }}</p> -->
    </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    // longest: 0,
      sequence: [],
      sequenceIndex:0, // this is used to move the sequence array forward 
      sequenceCountdown:1,
      taps: [],
      tapsIndex:0, // this is used in the check for pairs function
      greenBright: false,
      redBright: false,
      yellowBright: false,
      blueBright: false,
      canTap: false,
      turnTimer:false,
      tapCounter:0,
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

    // starts the playing sequence
    start: function() {
      this.sequence = [];
      this.taps = [];
      this.initializeSequence();
      this.playSequence();
    },
    reStart: function(){
      console.log("restart called");
      this.taps = [];
      console.log("taps was just reset");
      this.addToSequence();
      this.sequenceIndex=0;
      this.tapsIndex=0;
      this.sequenceCountdown=this.sequence.length;
      this.playSequence();

    },

    chooseRandomLight: function() {
        var index = Math.floor(Math.random() * 4);
        return this.lights[index];
    },

    initializeSequence: function(){
        for(var i=0; i<this.sequenceCountdown; i++){
          this.sequence.push(this.chooseRandomLight());
        }
    },
    // adds a random light to the sequence array
    addToSequence: function() {
        this.sequence.push(this.chooseRandomLight());
    //    console.log(this.sequence);
        
    },

    // calls the make bright function
    playSequence: function() {
         
         this.sequenceBright(this.sequenceIndex); // also sends makeBright the index in the sequence to use
        
    },

    // lights up specified box 
    sequenceBright: function(count){ // count is the index in the sequence the color resides in


          if(this.sequence[this.sequenceIndex]==='green'){
            this.greenBright=true;
          }
          else if(this.sequence[this.sequenceIndex]==='yellow'){
            this.yellowBright=true;
          }
          else if(this.sequence[this.sequenceIndex]==='red'){
            this.redBright=true;
          }
          else{
            this.blueBright=true;
          }
               
          // calls the makeDim function at 1 sec interval
          setTimeout(this.sequenceDim, 400);
      
    },

    // turns the light off
    sequenceDim: function(){
          if(this.greenBright){
              this.greenBright = !this.greenBright;
              console.log('green lit up');
          }
            else if(this.yellowBright){
                this.yellowBright = !this.yellowBright;
                console.log('yellow lit up');
            }
            else if(this.redBright){
              this.redBright = !this.redBright;
              console.log('red lit up');
            }
            else{
              this.blueBright = !this.blueBright;
              console.log('blue lit up');
            }

          this.sequenceCountdown--;
          
          // if there ARE MORE lights in the sequence
          if(this.sequenceCountdown>0){
            
            // move the index forward in the sequence
            this.sequenceIndex++;

            // call the RESTART function at one second
            setTimeout(this.playSequence, 400);     
           }
           
          // if there are NO MORE lights in the sequence
          else if(this.sequenceCountdown===0){
            this.canTap=true; // user can tap - maybe start a timer
           }
    },

    tapBright: function(color){
      switch(this.taps[this.tapsIndex]) {
    case 'red':
        this.redBright=true;
        break;
    case 'green':
        this.redBright=true;
        break;
    case 'yellow':
        this.yellowBright=true;
        break;
    case 'bue':
        this.blueBright=true;
        break;
    setTimeout(this.tapDim, 400);
  }
    },
    tapDim: function(){

    },

    captureTap: function(color) {
    

        // if CAN TAP is true : it's the players turn
        if(this.canTap){
        //  console.log( 'taps length[' + this.taps.length + '] tapsIndex[' + this.tapsIndex + ']');

          this.taps.push(color);
          this.tapsIndex++;
          this.tapsMeasure();
        }
      },
    tapsMeasure: function(){

      console.log("just entered tapsMeasure ");
      // if TAPS is as long as SEQUENCE
      if(this.tapsIndex===this.sequence.length){
        this.checkPairs();
        this.canTap=false;
        
       }
        
    },


    checkPairs: function(){
      var pairMatch=0;

      console.log( 'taps: ' + JSON.stringify( this.taps ) );
      console.log( 'sequence: ' + JSON.stringify( this.sequence ) );

      var sl = this.sequence.length;

      console.log("just entered checkPairs");
      console.log("1. sl: " + sl);
      console.log("2. pairMatch count: " + pairMatch);
      console.log("");

      for(var i=0; i<sl; i++){
        if(this.sequence[i]===this.taps[i]){
          pairMatch++;
          console.log(pairMatch + " match(s)");
        }
        else {
          console.log("mis-match");
          alert("You are a bad person!");
          this.destroyGame();
        }        
      }

      if(pairMatch===this.sequence.length){
          console.log('good - let\'s press restart');
          this.reStart();
        }
        else{
          console.log("this is the second else statement don't do anything");
        }         
    },

    startTimer: function() {
         setTimeout(this.timeout, 5000);
    },
    timeout: function(){
        if(this.turnTimer){
        }
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
