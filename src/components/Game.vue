<template>
  <h1>Test</h1>
  <table>
    <tr
      v-for="row of fields"
      :key="row"
    >
      <th
        v-for="cell of row"
        :key="cell"
      >
        <button
          class="game-cell"
          :class="{ uncovered: cell.uncovered }"
          :disabled="cell.uncovered"
          @click="clickCell(cell)"
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
  methods: {
    clickCell(cell) {
      cell.uncovered = true
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