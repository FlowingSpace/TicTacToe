

<template>
<div class="container">
<div class="grid-container">
<div class="line10">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          version="1.1"
          width="1000px"
          height="1000px"
        >
          <line
            x1="240"
            y1="50"
            x2="240"
            y2="600"
            style="stroke:rgb(255,255,255);stroke-width:2"
          />
          <line
            x1="480"
            y1="50"
            x2="480"
            y2="600"
            style="stroke:rgb(255,255,255);stroke-width:2"
          />
          <line
            x1="50"
            y1="200"
            x2="700"
            y2="200"
            style="stroke:rgb(255,255,255);stroke-width:2"
          />
          <line
            x1="50"
            y1="400"
            x2="700"
            y2="400"
            style="stroke:rgb(255,255,255);stroke-width:2"
          />
        </svg>
      </div>
        <div class="grid-item" id="0" @click="click(0,0)">&nbsp;</div>
        <div class="grid-item" id="1" @click="click(1,0)">&nbsp;</div>
        <div class="grid-item" id="2" @click="click(2,0)">&nbsp;</div>
        <div class="grid-item" id="3" @click="click(0,1)">&nbsp;</div>
        <div class="grid-item" id="4" @click="click(1,1)">&nbsp;</div>
        <div class="grid-item" id="5" @click="click(2,1)">&nbsp;</div>
        <div class="grid-item" id="6" @click="click(0,2)">&nbsp;</div>
        <div class="grid-item" id="7" @click="click(1,2)">&nbsp;</div>
        <div class="grid-item" id="8" @click="click(2,2)">&nbsp;</div>
      </div>
      
    </div>


  
</template>
      











<script>
export default {
  name: "constainer",
  data() {
    return {
      grid: [[0, 0, 0], [0, 0, 0], [0, 0, 0]],
      elements: [],
      winningdiag: []
    };
  },
  methods: {
    click: function(row, col) {
        var time = 0;

      //Get elements from DOM
      for (var x = 0; x < 9; x++) {
       this.elements.push(document.getElementById(x));
      }
      
      if(this.grid[row][col]==0){
        this.elements[row+3*col].innerHTML = "X";
        this.grid[row][col] = 1;
      }else{
        return;
      }


      //winCheck
      if(this.winCheck(row,col)==1){
        
        setTimeout(this.clear,time)
        alert("TODO PLAYER WON")
        
        return;
      }
      
      //Computer Move
      var totblank = 0;
      for(var i = 0;i<this.grid.length;i++){
        totblank+=this.grid[i].length - this.grid[i].filter((elem)=>!(elem===0)).length;
      }
     if(totblank==0){
        setTimeout(this.clear,time)
        alert("TODO TIE")
        return;
     }

      var numblank = Math.floor(Math.random()*(totblank-1))+1;
      var traversedblank = 0;
      var placed = false
       for(var i = 0;i<(this.grid.length);i++){
         if(placed){
           break;
         }
          for(var j = 0;j<(this.grid[i].length);j++){
          if(this.grid[i][j]===0){
            traversedblank += 1
          }
          if(traversedblank==numblank){
            this.elements[i+3*j].innerHTML="O"
            this.grid[i][j] = -1;
            placed = true;
            break;
          }
         }
      }
      if(!placed){
        alert("fQ")
        alert(numblank)
      }
      
      
      if(this.winCheck(row,col)==-1){
        setTimeout(this.clear,time)
        alert("TODO COMPUTER WON")
        return;
      }

    if(totblank==1){
        setTimeout(this.clear,time)
        alert("TODO TIE")
        return;
     }


    },

    winCheck: function(row, col) {

      //Check Diagonals
      if (!(this.grid[1][1] === 0)) {
        for (var i = 0; i < 4; i++) {
          //check opposite sides of the center
          if (this.grid[i % 3][Math.floor(i / 3)] === this.grid[2 - (i % 3)][2 - Math.floor(i / 3)] && !(this.grid[2 - (i % 3)][2 - Math.floor(i / 3)] === 0 || this.grid[i % 3][Math.floor(i / 3)] === 0)) {
            return (this.grid[i % 3][Math.floor(i / 3)]===1 && this.grid[1][1] === 1 ) ? 1 : ((this.grid[i % 3][Math.floor(i / 3)]===-1 && this.grid[1][1] === -1) ? -1 : null)
          }
        }
      }

      //Checks Horizontals and Vecticals from point just placed

      if (this.grid[row][0] === this.grid[row][1] && this.grid[row][1] === this.grid[row][2]) {
        return this.grid[row][0] === 1 ? 1 : (this.grid[row][0] === -1 ? -1 : null)
      }

      if (this.grid[0][col] === this.grid[1][col] && this.grid[1][col] === this.grid[2][col]) {
        return this.grid[0][col] === 1 ? 1 : (this.grid[0][col] === -1 ? -1 : null)
      }

    },

    clear: function(){
      this.grid = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
      for(var i = 0;i<this.elements.length;i++){
        this.elements[i].innerHTML="&nbsp;"
      }
    }
    
  }
};
</script>