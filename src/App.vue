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
      sequenceCountdown:3,
      taps: [],
      tapsIndex:0, // this is used in the check for pairs function
      greenBright: false,
      redBright: false,
      yellowBright: false,
      blueBright: false,
      canTap: false,
      turnTimer:false,
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
      this.initializeSequence();
      console.log("sequence length just after initialize" + this.sequence.length);  
      this.playSequence();
    //  this.startTimer();
    },
    reStart: function(){
      this.addToSequence();
      this.playSequence();
   //   this.startTimer();

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
        console.log(this.sequence);
        
    },

    // calls the make bright function
    playSequence: function() {
         
         this.makeBright(this.sequenceIndex); // also sends makeBright the index in the sequence to use
        
    },

    // lights up specified box 
    makeBright: function(count){ // count is the index in the sequence the color resides in

          console.log(this.sequenceIndex);

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
          setTimeout(this.makeDim, 1000);
      
    },

    // turns the light off
    makeDim: function(){
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

          this.sequenceCountdown--;
          
          // if there ARE MORE lights in the sequence
          if(this.sequenceCountdown>0){
            
            // move the index forward in the sequence
            this.sequenceIndex++;

            // call the RESTART function at one second
            setTimeout(this.playSequence, 1000);     
           }

          // if there are NO MORE lights in the sequence
          else if(this.sequenceCountdown===0){
            this.canTap=true; // user can tap - maybe start a timer
            //this.captureTap();
            //this.startTimer();
            //this.turnTimer=true; // allows timer to start
           }
    },

    captureTap: function(color) {
          console.log('taps ' + this.taps.length);
          // if CAN TAP is true : it's the players turn
          if(this.canTap){
            this.taps.push(color);
            console.log('added to taps: ' + this.taps.length);
            // if TAPS is as long as SEQUENCE
            if(this.taps.length===this.sequence.length){

              // check all pairs
              this.checkPairs();
              this.canTap=false;
              console.log('called check pairs');
             }
          }
      },


    checkPairs: function(){
        var pairMatch=0;

        console.log("sequence length" + this.sequence.length);
        
        for(var i=0; i<this.sequence.length; i++){
            
            // if the index of sequence matches the index of taps - move forward
            if(this.sequence[i]===this.taps[i]){
                pairMatch++;
                          }
                       }
           
                if(pairMatch===this.sequence.length){
                    alert('gotcha');
                  }
                  else{
                    alert('nope');
                  }


        //           this.turnTimer=false;
        //           clearTimeout(this.startTimer);
        //           alert('good job... lets make this harder');
        //           this.reStart(); // call restart
        //           break;
        //           }
        //           else{
        //             console.log('do nothing');
        //           }
        //      }


        //     else{
        //       alert('nope');
        //       this.turnTimer=false;
        //       break;
        //     }
        //     }

        // }
               
    },

    startTimer: function() {
         setTimeout(this.timeout, 5000);
    },
    timeout: function(){
        if(this.turnTimer){
       // alert('time\'s up');
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
