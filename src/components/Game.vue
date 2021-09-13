<template>
  <h1>Test</h1>
  <table @keydown="keyPressed($event)">
    <tr
      v-for="row of fields"
      :key="row"
    >
      <th
        v-for="cell of row"
        :key="cell"
      >
        <button
          :ref="`${cell.x}:${cell.y}`"
          class="game-cell"
          :class="{ uncovered: cell.uncovered }"
          @click="clickCell(cell)"
          @focus="focus({x: cell.x, y: cell.y})"
        >
          {{ cell.x }}:{{ cell.y }}
        </button>
      </th>
    </tr>
  </table>
</template>

<script>
import { defineComponent } from '@vue/composition-api'
import { reactive } from 'vue'

export default defineComponent({
  props: {
    width: {
      type: Number,
      default: 5,
    },
    height: {
      type: Number,
      default: 5,
    },
  },
  setup(props) {
    let fields = []
    for (let y = 0; y < props.height; ++y) {
      fields.push([])
      for (let x = 0; x < props.width; ++x) {
        fields[y].push(reactive({
          x, y,
          uncovered: false,
          content: 'dirt',
        }))
      }
    }
    return {
      fields,
    }
  },
  data() {
    return {
      selected: {
        x: -1, y: -1,
      },
    }
  },
  methods: {
    clickCell(cell) {
      cell.uncovered = true
    },
    keyPressed($event) {
      switch($event.key) {
        case 'ArrowDown':
        case 's':
          this.selected.y++
          break
        case 'ArrowUp':
        case 'w':
          this.selected.y--
          break
        case 'ArrowRight':
        case 'd':
          this.selected.x++
          break
        case 'ArrowLeft':
        case 'a':
          this.selected.x--
          break
      }
      this.selected.x = Math.min(Math.max(this.selected.x, 0), this.width - 1)
      this.selected.y = Math.min(Math.max(this.selected.y, 0), this.height - 1)
      this.$refs[`${this.selected.x}:${this.selected.y}`].focus()
    },
    focus(coords) {
      this.selected = coords
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