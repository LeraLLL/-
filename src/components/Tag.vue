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
          :class="`cell ${cell === 0 && 'hide'}`"
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
      model: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 0],
      counter: 0,
      victory: false,
      start: new Date(),
      now: new Date()
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
      return array.sort(() => Math.random() - 0.5);
    },
    checkResult() {
      let result = 0;
      this.model.forEach((item, index) => {
        item == this.cells[index] && result++;
      })
      if (result > 15) this.victory = true
    },
    moveCell(index) {
      let valueCell = this.cells[index];
      if (this.cells[index - 1] == 0) {
        this.cells[index] = 0;
        this.cells[index - 1] = valueCell;
        this.counter++
      } else if (this.cells[index - 4] == 0) {
        this.cells[index] = 0;
        this.cells[index - 4] = valueCell;
        this.counter++
      } else if (this.cells[index + 1] == 0) {
        this.cells[index] = 0;
        this.cells[index + 1] = valueCell;
        this.counter++
      } else if (this.cells[index + 4] == 0) {
        this.cells[index] = 0;
        this.cells[index + 4] = valueCell;
        this.counter++
      }
      this.checkResult();
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

    p{
      margin: 0;
      font-weight: bold;
      font-size: .8rem;
      line-height: 140%;
      text-align: center;
    }
  }
}

.gameBox {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 1rem;
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