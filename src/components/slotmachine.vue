<template>
<div class="slot_kast">
  <div class="slot-machine">
    <button @click="start">start</button>
    <div class="slot" :key="i" v-for="(slot,i) in slots" ref="slots">
      <h2>{{ slot.title }}</h2>
      <div class="slot__window">
        <div class="slot__wrap">
          <div class="slot__item" :key="i" v-for="(opt,i) in slot.items"> <img v-bind:src="'/Images/' + opt" />  </div>
          <div class="slot__item slot__item--copy">{{ slot[0] }}</div>
        </div>
      </div>
    </div>
  </div>
    <div class="slot_arm">
    </div>
    <div class="slot_arm2">
    </div>
    <div class="message">
      Om deze game te spelen moet het scherm een minimale grootte hebben van 800px
    </div>
  </div>

  
</template>

<script>
const next = window.requestAnimationFrame ||
   window.webkitRequestAnimationFrame ||
   window.mozRequestAnimationFrame ||
   window.msRequestAnimationFrame ||
   window.oRequestAnimationFrame ||
   function(cb) { window.setTimeout(cb, 1000/60) }
export default {
    name: "slotmachine",
    data: function() {
  	return {
      slots: [{
        title: "SLOT 1",
        items: [
          "aardbei.png",
          "banaan.png",
          "druif.png",
          "watermelon.png",
        ]
      }, {
      	title: "SLOT 2",
        items: [
          "aardbei.png",
          "banaan.png",
          "druif.png",
          "watermelon.png",
        ]
      }, {
      	title: "SLOT 3",
        items: [
          "aardbei.png",
          "banaan.png",
          "druif.png",
          "watermelon.png",
        ]
      }, 

      ],
      opts: null,
      startedAt: null
    }
  },
  methods: {
  	start: function() {
    	if (this.opts) {
      	return
      }
    	  		
      this.opts = this.slots.map( (data, i) => {
        const slot = this.$refs.slots[i]
        const choice = Math.floor( Math.random() * this.slots.length )
        console.log("choice", i, this.slots[choice])

        const opts = {
        	el: slot.querySelector('.slot__wrap'),
          finalPos: choice * 180,
          startOffset: 2000 + Math.random() * 500 + i * 500,
          height: this.slots.length * 180,
          duration: 3000 + i * 700, // milliseconds
          isFinished: false,
        }
        
        return opts
        
      })
      
      next( this.animate )
      
  	},
    
    animate: function(timestamp) {

			if (this.startedAt == null) {
      	this.startedAt = timestamp
      }
      
      const timeDiff = timestamp - this.startedAt
      
      this.opts.forEach( opt => {
      
      	if (opt.isFinished) {
        	return
        }
      		
        const timeRemaining = Math.max(opt.duration - timeDiff, 0)
        const power = 3
        const offset = ( Math.pow(timeRemaining, power) / Math.pow(opt.duration, power) ) * opt.startOffset
        
        // negative, such that slots move from top to bottom
        const pos = -1 * Math.floor((offset + opt.finalPos) % opt.height)
        
        opt.el.style.transform = "translateY(" + pos + "px)"
        
        if ( timeDiff > opt.duration ) {
          console.log('finished', opt, pos, opt.finalPost)
        	opt.isFinished = true
        }
        
      })
      
      if (this.opts.every( o => o.isFinished )) {
      	this.opts = null
        this.startedAt = null
        console.log('finished')
        //if
      } else {
        next( this.animate )
      }

    }
    
    
  }
}
</script>

<style>
body{
  background-color: black;  
}
.slot {
  float: left;
  margin: 0.4em;
}

.slot__window {
    background-color: lightgrey;
    border: solid black 4px;
    height: 200px;
    overflow: hidden;
}

.slot__wrap {

}
.slot__item {
    height: 178px;
    background-color: #01255f;
}


.slot__item--copy {
}

img {
    width: 200px;
    height: 200px;
    position: relative;
    top: 0;
    left: 0px;
    background-color: #01255f;
}
.slot_kast {
    height: 364px;
    background-color: red!important;
    z-index: 999;
    margin: auto;
    width: 705px;
}
button {
    position: relative;
    top: 48px!important;
    left: 370px;
    z-index: 999999;
    background-color: darkred;
    border: none;
    height: 70px;
    width: 70px;
    border-radius: 50%;
    display: -webkit-box;
    margin: auto;
}
.slot_arm {
    width: 10px;
    height: 130px;
    background-color: grey;
    position: relative;
    top: 37px;
    left: 717px;
}
.slot_arm2 {
    width: 80px;
    height: 100px;
    background-color: grey;
    position: relative;
    left: 705px;
    top: 31px;
}
.message{
  display:none;
}
@media only screen and (max-width: 800px) {
  .slot_arm, .slot_arm2, .slot-machine{
    display:none;
  }
  .message{
    display:inline-block;
    font-size:30px;
  }
  .slot_getal{
    display: none;
  }
}

.slot_getal {
    width: 180px;
    height: 120px;
    z-index: 999;
    position: relative;
    top: 64px;
    background-color: red;
    color: purple;
}

</style>
