<template>
  <div class="timest" >
    <hr>
      <div class="thetable">
        <div class="row">
          <div v-for="(h,indexh) in [1,2,3,4,5,6,7,8,9,10]" :key="indexh" class="cells headings">
            {{h}}
          </div>
        </div>
        <div v-for="(row,indexj) in items" :key="indexj" class="row"> 
          <div class="cells headings">{{indexj+2}}</div>
          <div v-for="(cell,indexi) in row" :key="indexi" class="cells" :class="{ selectedcell: cell.isLighted }">
            <span v-show="cell.isVisible"> {{ cell.value }} </span>
          </div>
        </div>
      </div>
    <hr>
    <div class="display">{{ enteredString || '0' }}</div>
    <div @click="clickkey" class="keyscontainer">
    <div class="keys">1</div>
    <div class="keys">2</div>
    <div class="keys">3</div>
    <div class="keys">4</div>
    <div class="keys">5</div>
    <div class="keys">6</div>
    <div class="keys">7</div>
    <div class="keys">8</div>
    <div class="keys">9</div>
    <div class="keys">Start</div>
    <div class="keys">0</div>
    <div class="keys">Del</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TimesT',
  //props: { msg: String },
  data() {
    return { enteredString: '', j: 0, i: 0, items: [] }
  },
  computed: {
    highLighted: {
      get: function () {
        return this.items[this.j][this.i].isLighted;
      },
      set: function (newValue){
        this.items[this.j][this.i].isLighted=newValue;
      }
    },
    currentCell: function() {
      return this.items[this.j][this.i]
    }
  },
  created() {
    for (let j=2; j<11; j++) {
      let row = []
      for (let i=2; i<11; i++) row.push( {
          value: String(j*i),
          nHits: 0,
          nMistakes: 0,
          isLighted: false,
          isVisible: true
          } )
      this.items.push(row);
    }
    window.addEventListener('keydown', this.keydown);
  },

  beforeDestroy() {
    window.removeEventListener('keydown', this.keydown);
  },
  methods: {

    clickkey(event) {
      if ("0123456789".includes(event.target.textContent)) {
        this.enteredString+=event.target.textContent;
        this.checkEntered();
      } else if (event.target.textContent === 'Del') {
        this.enteredString=this.enteredString.slice(0,-1);
      } else if (event.target.textContent == 'Start') {
        this.reset();
      }
    },

    keydown(event) {
      if ('0123456789'.includes(event.key)) {
        this.enteredString+=event.key;
        this.checkEntered();
      }
      else if (event.key === 'Delete' || event.key === 'Backspace') {
        this.enteredString=this.enteredString.slice(0,-1);
        event.preventDefault();
      }
    },

    showcell(i,j) {
      this.items[j][i].isVisible=true;
    },

    reset () {
      this.highLighted=false;
      this.i=0;
      this.j=0;
      for (let row of this.items) {
        for (let cell of row) {
          if (cell.isVisible) cell.isVisible=false;
          if (cell.highLighted) cell.highLighted=false;
        }
      }
      this.currentCell.isLighted=true;
    },

    checkEntered() {
      let i=this.i, j=this.j
      if (this.currentCell.value === this.enteredString) {
        this.showcell(i,j);
        this.highLighted=false;
        if(i!=j) this.showcell(j,i);
        if (i<8) {
          this.i++;
          this.highLighted=true;
        } else if (j<8) {
          this.j++;
          this.i=this.j;
          this.highLighted=true
        }     
        this.enteredString='';

      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.timest {
  width: 320px;
  margin: 1px auto;
}
.keyscontainer {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(40px,auto);
}
.keys {
    display: flex;
    justify-content: center;
    align-items: center;
    border:1px solid darkgray;
}
.display {
  grid-column: 1 / 5;
  border: 1px solid darkgray;
}

.thetable {
  border:1px solid darkgray;
  font-size: 0.7em;
}

.row {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
}

.cells{
  border:1px solid darkgray;
  background-color: lightgreen;
}

.selectedcell{
    background-color: yellow;
}

.headings {
  background-color: lightsteelblue;
}

</style>
