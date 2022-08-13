<template>

  <div class="verify">
    <div class="verify-result" :class="{show:result !==null,success:result,error:!result}">
      <div class="wrap">
        <p>{{ result ? '验证成功':'验证失败' }}</p>
        <span @click="display" class="verify-btn" v-show="!result">再试一次</span>
      </div>
    </div>
    <div class="verify-title">{{title}}</div>
    <div class="verify-grid-wrap">
      <div class="verify-grid" v-for="(grid,index) in gridView" :key="index">
        <div class="verify-grid-item" :style="'background:'+grid.value" v-for="(item,idx) in grid[mode]" @click="select(grid.key,item.key)" :key="idx">
          {{ mode === 'shape' ? item.name : '' }}
        </div>
      </div>
    </div>
  </div>

</template>

<script>
const position = [
  { key: 1, name: '左上方位置' },
  { key: 2, name: '正上方位置' },
  { key: 3, name: '右上方位置' },
  { key: 4, name: '左边位置' },
  { key: 5, name: '中间位置' },
  { key: 6, name: '右边位置' },
  { key: 7, name: '左下方位置' },
  { key: 8, name: '正下方位置' },
  { key: 9, name: '右下方位置' }
]
const shape = [
  { key: 1, name: '❈' },
  { key: 2, name: '✿' },
  { key: 3, name: '✲' },
  { key: 4, name: '♠' },
  { key: 5, name: '❀' },
  { key: 6, name: '☂' },
  { key: 7, name: '♥' },
  { key: 8, name: '✪' },
  { key: 9, name: '♣' }
]
export default {
  props: {
    mode: {
      type: String,
      default: 'shape'
    },
    grid: {
      type: Array,
      default: [
        { key: 1, value: '#f92e2e', name: '红色' },
        { key: 2, value: '#03c144', name: '绿色' },
        { key: 3, value: '#1b96ff', name: '蓝色' }
      ]
    }
  },
  data() {
    return {
      gridView: [],
      title: '',
      result: null,
      randomGrid: null,
      randomItem: null
    }
  },
  created() {
    // 初始化验证
    this.display();
  },
  methods: {
    display() {
      this.result = null

      this.gridView = this.getRandomArray(this.grid)

      for (let i = 0; i < this.gridView.length; i++) {
        this.gridView[i][this.mode] = this.mode === 'position' ? position : this.getRandomArray(shape)
      }

      //随机选取一个区域与位置
      this.randomGrid = this.gridView[Math.floor(Math.random() * this.gridView.length)]
      this.randomItem = this.randomGrid[this.mode][Math.floor(Math.random() * this.randomGrid[this.mode].length)]
      this.title = `请点击${this.randomGrid.name}区域的${this.randomItem.name}`
    },

    // 将数组内的元素随机打乱
    getRandomArray(arr) {
      for (let i = 0; i < arr.length; i++) {
        let ramdomNum = Math.floor(Math.random() * arr.length)
        arr.splice(i, 1, ...arr.splice(ramdomNum, 1, arr[i]));
      }
      return [...arr]
    },

    select(a, b) {
      this.result = this.randomGrid.key === a && this.randomItem.key === b
      this.$emit('result', this.result)
    }
  }
}
</script>

<style scoped>
.verify {
  background: #fff;
  position: relative;
  display: inline-block;
  text-align: center;
  font-size: 15px;
  overflow: hidden;
}
.verify .verify-title {
  padding: 10px 0;
}
.verify .verify-grid-wrap {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1px;
}
.verify .verify-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  grid-gap: 1px;
}
.verify .verify-grid-item {
  padding: 5px;
  font-size: 18px;
  cursor: pointer;
  color: #fff;
  min-height: 20px;
  transition: all 0.3s ease-out;
}
.verify .verify-grid-item:hover {
  transform: scale(0.9);
}

.verify .verify-result {
  position: absolute;
  z-index: 2;
  width: 100%;
  height: 100%;
  left: 0;
  box-sizing: border-box;
  padding: 20px;
  background: rgba(255, 255, 255, 0.9);
  text-align: center;
  font-family: Arail;
  top: -200%;
  -webkit-transition: 0.3s ease 0s;
  -moz-transition: 0.3s ease 0s;
  -ms-transition: 0.3s ease 0s;
  transition: 0.3s ease 0s;
  display: flex;
  align-items: center;
  justify-content: center;
}
.verify .verify-result .wrap {
  text-align: center;
}
.verify .verify-result p {
  font-size: 25px;
  font-weight: bold;
}
.verify .verify-result.success p {
  color: #116311;
}
.verify .verify-result.error p {
  color: #8b0f0f;
}
.verify .verify-result .verify-btn {
  border: 1px solid #ccc;
  padding: 3px 10px;
  display: inline-block;
  border-radius: 3px;
  font-size: 13px;
  margin-top: 20px;
  cursor: pointer;
}
.verify .verify-result.show {
  top: 0;
}
</style>