<template>
  <div class="timest">
    <div class="display">{{current}}</div>
    <div v-on:click="click" class="keyscontainer">
    <div class="keys">1</div>
    <div class="keys">2</div>
    <div class="keys">3</div>
    <div class="keys">&times;</div>
    <div class="keys">4</div>
    <div class="keys">5</div>
    <div class="keys">6</div>
    <div class="keys">&div;</div>
    <div class="keys">7</div>
    <div class="keys">8</div>
    <div class="keys">9</div>
    <div class="keys">&plus;</div>
    <div class="keys">0</div>
    <div class="keys">Reset</div>
    <div class="keys">Del</div>
    <div class="keys">&minus;</div>
    </div>
    <hr>
      <div class="thetable">
        <div class="row">
          <div class="cells headings">
          </div>
          <div v-for="(h,indexh) in [2,3,4,5,6,7,8,9]" :key="indexh" class="cells headings">
            {{h}}
          </div>
        </div>
        <div v-for="(row,indexi) in items" :key="indexi" class="row"> 
          <div class="cells headings">{{indexi+2}}</div>
          <div v-for="(cell,indexj) in row" :key="indexj" class="cells">
            {{ cell }}
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'TimesT',
  props: {
    msg: String
  },
  data() {
    return {
      current: '123',
      items: []
    }
  },
  created() {
    
    for (let j=2; j<10; j++) {
      let row = []
      for (let i=2; i<10; i++) {
        row.push(j*i);        
      }
      this.items.push(row);
    }
  },
  methods: {
    click(event) {
      console.log(event);
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
  grid-template-columns: repeat(4, 1fr);
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
  font-size: 0.8em;
}

.row {
  display: grid;
  grid-template-columns: repeat(9, 1fr);
}

.cells{
  border:1px solid darkgray;
  background-color: lightgreen;
}

.headings {
  background-color: lightsteelblue;
}

</style>
