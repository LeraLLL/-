<template>
  <div class="wrapper">
    <div class="header">
      <div class="name">15</div>

      <div class="container">
        <p>Ходов</p>
        <p>{{counter}}</p>
      </div>

      <div v-if="timer" class="container">
        <p>Время</p>
        <p>{{timer}}</p>
      </div>
    </div>

    <div class="gameBox">
      <button
          :class="`cell ${cell === 0 ? 'hide' : ''}`"
          @click="moveCell(index)"
          v-for="(cell, index) in cells"
          :key="index"
      >
        {{cell !== 0 ? cell : ''}}
      </button>
    </div>

    <div v-if="victory" class="victoryBlock">
      <h1>Вы победили!</h1>
      <button
          @click="throwOff"
          class="btn"
      >
        play again
      </button>
    </div>

    <div v-else class="throw">
      <button
          @click="throwOff"
          class="btn"
      >
      throw off
      </button>
    </div>
  </div>
</template>s

<script>
import moment from 'moment'

export default {
  name: "Tag",
  created() {
    return this.shuffle(this.cells);
  },
  mounted () {
    setInterval(() => {
      this.now = new Date()
    }, 1000);
  },
  data() {
    return {
      cells: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 0],
      counter: 0,
      victory: false,
      start: new Date(),
      now: new Date(),
      move: {up: -4, left: -1, down: 4, right: 1}
    };
  },
  computed: {
    timer() {
      if(this.now) {
        const time = this.now - this.start
        return  moment(time).format('mm:ss')
      } else {
        return null
      }
    }
  },
  methods: {
    shuffle(array) {
      const shuffleArray = array.sort(() => Math.random() - 0.5)
      if(this.solvable(shuffleArray)) return array.sort(() => Math.random() - 0.5)
      else this.shuffle(shuffleArray)
    },
    checkResult() {
      let result = 0;
      this.cells.forEach((item, index) => {
        item === index+1 && result++;
      })
      if (result > 15) this.victory = true
    },
    moveCell(index) {
      let valueCell = this.cells[index];
      Object.keys(this.move).forEach(key => {
        const to = index + this.move[key]
        if(this.cells[to]=== 0) {
          this.cells[index] = 0;
          this.cells[to] = valueCell;
          this.counter++
        }
      })
      this.checkResult();
    },
    solvable(array) {
      for (var kDisorder = 0, i = 1, len = array.length-1; i < len; i++)
        for (let j = i-1; j >= 0; j--) if (array[j] > array[i]) kDisorder++;
      return !(kDisorder % 2);
    },
    throwOff() {
      this.victory = false
      this.shuffle(this.cells)
      this.counter = 0
      this.start = new Date()
      this.now = new Date()
    }
  },
}
</script>

<style scoped lang="scss">

.wrapper {
  width: fit-content;
  height: fit-content;
  margin: 0 auto;
}
.header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;

  .name {
    font-weight: bold;
    font-size: 3rem;
    color: #6C6772;
  }

  .container {
    background: #A8A0B8;
    border-radius: .4rem;
    padding: .5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    color: white;

    p{
      margin: 0;
      font-weight: bold;
      font-size: 1.2rem;
      line-height: 140%;
      text-align: center;
    }
  }
}

.gameBox {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: .4rem;
  width: fit-content;
  height: fit-content;
  padding: .5rem;
  background: #A8A0B8;
  border-radius: .4rem;
}

.cell{
  background: #D6CCE4;
  border: .1rem solid transparent;
  border-radius: .4rem;
  font-size: 2rem;
  font-weight: bold;
  color: #696273;
  outline: none;
  padding: .8rem;

  &.hide {
    background: transparent;
  }
}

.victoryBlock {
  display: flex;
  justify-content: space-between;
}

.throw, .victoryBlock {
  margin-top: 1rem;
}

.btn {
  outline: none;
  padding: .5rem 1rem;
  background: #696273;
  border: none;
  font-size: 1rem;
  color: white;
  border-radius: .4rem;
  cursor: pointer;
}
</style>