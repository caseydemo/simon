<template>
  <div id="app">

    <h1>Simon</h1>
    <div id="simon">
        <div class="display">
          <div id="turnIndicator" class="panel panel-danger" v-bind:class="{'playerTurn':!canTap}">
            <div class="panel-heading">
            <h3 class="panel-title">Your Turn!</h3>
          </div>
        </div>
    </div>


    <div class="sequence-display">{{sequenceDisplay}}</div>

    <div class="row">
        <div id="green" class="light col" v-bind:class="{'bright':greenBright}" v-on:click="captureTap('green')">
        </div>
        <div id="red" class="light col" v-bind:class="{'bright':redBright}" v-on:click="captureTap('red')"></div>
      </div>
      <div class="row">
        <div id="yellow" class="light col" v-bind:class="{'bright':yellowBright}" v-on:click="captureTap('yellow')">
        </div>
        <div id="blue" class="light col" v-bind:class="{'bright':blueBright}" v-on:click="captureTap('blue')">
        </div>
      </div>
    </div>

    <div id="controls" class="row">
      <div class="col"><button class="start" v-on:click="start">Start</button></div>
    </div>

    <div id="history">
    </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    // longest: 0,
     //playerTurn:false,
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
      sequenceCounter:0,
      sequenceDisplay:0,
      turnMessage: 'it\'s your turn!',
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
      this.taps = [];
      this.initializeSequence();
      this.playSequence();
    },
    reStart: function(){
      this.tapCounter=0;
      this.sequenceCounter=0;
      this.taps = [];
      this.addToSequence();
      this.sequenceIndex=0;
      this.tapsIndex=0;
      this.sequenceCountdown=this.sequence.length;
      this.playSequence();

    },
    destroyGame: function(){
        this.tapCounter=0;
        this.sequenceCounter=0;
        this.canTap=false;
        this.sequenceIndex=0;
        this.tapsIndex=0;
        this.sequenceCountdown=1;
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
        console.log( 'sequence: ' + JSON.stringify( this.sequence ) );

        
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
              // console.log('green lit up');
          }
            else if(this.yellowBright){
                this.yellowBright = !this.yellowBright;
                // console.log('yellow lit up');
            }
            else if(this.redBright){
              this.redBright = !this.redBright;
              // console.log('red lit up');
            }
            else{
              this.blueBright = !this.blueBright;
              // console.log('blue lit up');
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
            this.sequenceDisplay=this.sequence.length;
           }
    },

    tapBright: function(color){

    //  console.log('tapbright called');
      
      switch(color) {
    case 'red':
        this.redBright=true;
        setTimeout(this.tapDim, 200);
        break;
    case 'green':
        this.greenBright=true;
      //  console.log("green should have lit up");
        setTimeout(this.tapDim, 200);
        break;
    case 'yellow':
        this.yellowBright=true;
        setTimeout(this.tapDim, 200);
        break;
    case 'blue':
        this.blueBright=true;
      //  console.log('blue should be lit up');
        setTimeout(this.tapDim, 200);
        break;
    
  }
    },
    tapDim: function(){
      this.redBright=false;
      this.greenBright=false;
      this.yellowBright=false;
      this.blueBright=false;

      setTimeout(this.firstCheck, 200);
      // console.log('called first check');
    },

    captureTap: function(color) {
    

        // if CAN TAP is true : it's the players turn
        if(this.canTap){
        //  console.log( 'taps length[' + this.taps.length + '] tapsIndex[' + this.tapsIndex + ']');
          this.tapBright(color);
          this.taps.push(color);
          this.tapsIndex++;
        //  console.log("taps index " + this.tapsIndex);
          
          // check as we add each element
          // console.log('this is the whole taps array: ');
          console.log( 'taps: ' + JSON.stringify( this.taps ) );

          // console.log('this is the whole sequence array: ');
          console.log( 'sequence: ' + JSON.stringify( this.sequence ) );

          // console.log("this is what we just passed in: " + this.taps[this.taps.length-1]);
          // console.log(this.taps[this.taps.length-1] + ", " + this.sequence[this.tapsIndex]);

        }
      },
      firstCheck: function(){
        // console.log('just entered first check');

        console.log('this.taps: ' + this.taps[this.tapCounter]);
        console.log('this.sequence: ' + this.sequence[this.sequenceCounter]);

          if(this.taps[this.tapCounter]===this.sequence[this.sequenceCounter]){

            this.tapsMeasure();

          }
          else{
            alert('nope. wrong. your bad');
            this.destroyGame();
          }
          
        
      },
    tapsMeasure: function(){

      // console.log("just entered tapsMeasure ");
      // if TAPS is as long as SEQUENCE
      if(this.tapsIndex===this.sequence.length){
        this.checkPairs();
        this.canTap=false;
        
       }
        
    },


    checkPairs: function(){
      var pairMatch=0;

    //  console.log( 'taps: ' + JSON.stringify( this.taps ) );
    //  console.log( 'sequence: ' + JSON.stringify( this.sequence ) );

      var sl = this.sequence.length;

    //  console.log("just entered checkPairs");
    //  console.log("1. sl: " + sl);
    //  console.log("2. pairMatch count: " + pairMatch);
    //  console.log("");

      for(var i=0; i<sl; i++){
        if(this.sequence[i]===this.taps[i]){
          pairMatch++;
    //      console.log(pairMatch + " match(s)");
        }
        else {
          console.log("mis-match");
          alert("You are a bad person!");
          this.destroyGame();
        }        
      }

      if(pairMatch===this.sequence.length){
    //      console.log('good - let\'s press restart');
          this.reStart();
        }
        else{
    //      console.log("this is the second else statement don't do anything");
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
body{
  background-image: url("../sky-background.jpg");

}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 400px;
  margin-right: 450px;
   color:white;
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
  border: 5px solid #000;
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

.playerTurn{
  display:none;
}

.display{
  margin: auto;
  width: 100px;
  height: 60px;
  border:solid;

}



</style>
