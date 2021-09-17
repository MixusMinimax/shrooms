<template>
  <h1>Test</h1>
  <Field
    :cells="cells"
    :width="width"
    :height="height"
    :get-cell-classes="cell => ({ uncovered: cell.uncovered })"
    :on-cell-click="onCellClick"
  />
  <Field
    :cells="cells"
    :width="width"
    :height="height"
    :get-cell-classes="cell => ({ uncovered: cell.uncovered })"
    :on-cell-click="onCellClick"
  />
</template>

<script>
import { defineComponent } from '@vue/composition-api'
import Field from './Field'
import { reactive } from 'vue'

function createCells(width, height, cellTemplate) {
  let cells = []
  for (let y = 0; y < height; ++y) {
    cells.push([])
    for (let x = 0; x < width; ++x) {
      cells[y].push(reactive({ x, y, ...cellTemplate }))
    }
  }
  return cells
}

export default defineComponent({
  name: 'Game',
  components: {
    Field,
  },
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
    return {
      cells: createCells(props.width, props.height, { uncovered: false }),
    }
  },
  methods: {
    onCellClick(cell) {
      cell.uncovered = true
    },
  },
})
</script>

<style lang="scss">
table + table {
  margin-left: 20px;
}
table {
  display: inline-block;
  > tr > th {
    > button, > button[disabled], > button:disabled {
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
</style>