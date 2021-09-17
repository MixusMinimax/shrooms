<template>
  <table @keydown="keyPressed($event)">
    <tr
      v-for="row of cellsRef.value"
      :key="row"
    >
      <th
        v-for="cell of row"
        :key="cell"
      >
        <button
          :ref="`${cell.x}:${cell.y}`"
          class="game-cell"
          :class="getCellClasses(cell)"
          @click="onCellClick(cell)"
          @focus="focus(cell)"
        >
          {{ cell.x }}:{{ cell.y }}
        </button>
      </th>
    </tr>
  </table>
</template>

<script>
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  name: 'Field',
  props: {
    width: {
      type: Number,
      default: 5,
    },
    height: {
      type: Number,
      default: 5,
    },
    cellsRef: {
      type: Object,
      default: new Object(),
    },
    onCellClick: {
      type: Function,
      default() {},
    },
    getCellClasses: {
      type: Function,
      default() {},
    },
    cellTemplate: {
      type: Object,
      default: new Object(),
    },
  },
  setup(props) {
    let fields = []
    for (let y = 0; y < props.height; ++y) {
      fields.push([])
      for (let x = 0; x < props.width; ++x) {
        fields[y].push(reactive({ x, y, ...cellTemplate }))
      }
    }
    cellsRef.value = fields
  },
  data() {
    return {
      selected: {
        x: -1, y: -1,
      },
    }
  },
  methods: {
    keyPressed($event) {
      switch($event.key) {
        case 'ArrowDown':
        case 's':
        case 'j':
          this.selected.y++
          break
        case 'ArrowUp':
        case 'w':
        case 'k':
          this.selected.y--
          break
        case 'ArrowRight':
        case 'd':
        case 'l':
          this.selected.x++
          break
        case 'ArrowLeft':
        case 'a':
        case 'h':
          this.selected.x--
          break
      }
      this.selected.x = Math.min(Math.max(this.selected.x, 0), this.width - 1)
      this.selected.y = Math.min(Math.max(this.selected.y, 0), this.height - 1)
      this.$refs[`${this.selected.x}:${this.selected.y}`].focus()
    },
    focus(cell) {
      this.selected = { x: cell.x, y: cell.y }
    },
  },
})
</script>

<style scoped lang="scss">
table {
  border-collapse: collapse;
  > tr {
    padding: 0;
    > th {
      padding: 0;
      > button, > button[disabled], > button:disabled {
        position: relative;
        color: white;
        width: 50px;
        height: 50px;
        border-width: 0;
        margin: 0;
        background-color: red;
        &:focus {
          z-index: 10;
          outline-style: solid;
          outline-width: 4px;
          outline-color: rgb(116, 116, 235);
        }
        &.uncovered {
          background-color: green;
        }
        &:not(.uncovered) {
          &:hover{
            background-color: rgb(255, 73, 73);
          }
          &:active{
            background-color: rgb(184, 45, 45);
          }
        }
      }
    }
  }
}
</style>