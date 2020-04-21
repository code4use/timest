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
          <div ref="refDiv" @click="clickcell(indexi,indexj,$event)" v-for="(cell,indexi) in row" :key="indexi" class="cells">
            <span ref="refCell" v-show="cell.isVisible"> {{ cell.value }} </span>
          </div>
        </div>
      </div>
    <hr>
    <div class="display">{{ current || '0' }}</div>
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
    <div class="keys">Reset</div>
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
    return { current: '', j: 0, i: 0, items: [] }
  },
  created() {
    for (let j=2; j<11; j++) {
      let row = []
      for (let i=2; i<11; i++) row.push( {
          value: String(j*i),
          nHits: 0,
          nMistakes: 0,
          isVisible: false 
          } )
      this.items.push(row);
    }
    window.addEventListener('keydown', this.keydown);
  },
  mounted() {
  //  console.log(this.$refs.refCell)
    this.reset();
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.keydown);
  },
  methods: {
    clickkey(event) {
      if ("0123456789".includes(event.target.textContent)) {
        this.current+=event.target.textContent;
        this.checkEntered();
      } else if (event.target.textContent === 'Del') {
        this.current=this.current.slice(0,-1);
      } else if (event.target.textContent == 'Reset') {
        this.reset();
      }

    },
    keydown(event) {
      if ('0123456789'.includes(event.key)) {
        this.current+=event.key;
        this.checkEntered();
      }
      else if (event.key === 'Delete' || event.key === 'Backspace')
        this.current=this.current.slice(0,-1);
    },
    // eslint-disable-next-line no-unused-vars
    clickcell(i,j,event) {
      this.showcell(i,j);
    },
    showcell(i,j) {
      this.items[j][i].isVisible=true;
      //if(i!=j) this.items[i][j].isVisible=true;    
    },
    reset () {
      this.i=0;
      this.j=0;
      this.$refs.refDiv[this.i+this.j*9].classList.add('selectedcell');
      for(let j=0; j<9; j++) {
        for(let i=0; i<9; i++) {
          if( this.items[j][i].isVisible ) this.items[j][i].isVisible=false;
        }
      }
    },
    checkEntered() {
      let i=this.i, j=this.j
      if (this.items[j][i].value === this.current) {
        this.showcell(i,j);
        this.$refs.refDiv[i+j*9].classList.toggle('selectedcell');
        if(i!=j) this.showcell(j,i);
        if (i<8) {
          this.i++;
          this.$refs.refDiv[this.i+this.j*9].classList.toggle('selectedcell');
        } else if (j<8) {
          this.j++;
          this.i=this.j;
          this.$refs.refDiv[this.i+this.j*9].classList.toggle('selectedcell');
        }     
        this.current='';

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
