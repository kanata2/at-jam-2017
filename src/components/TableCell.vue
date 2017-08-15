<template>
  <div
    class="cell"
    v-on:click="toggleSelectStatus"
    :style="styleProps">
    {{ turn.start | hhmmWithDelimiter }}-{{ turn.end | hhmmWithDelimiter }} ({{ this.time() }}) <br>
    {{ turn.lineup.map((e, i, a) => e.name).join('/') || turn.replacement }}
  </div>
</template>

<script>
  export default {
    name: 'table-cell',
    props: ['turn', 'color'],
    data () {
      return {
        styleProps: {
          height: `${this.time() / 2 * 10}px`,
          'margin-top': `${this.turn.offset}px`,
          'border-bottom': `5px solid ${this.color}`,
        },
      }
    },
    methods: {
      toggleSelectStatus (e) {
        //const pos = e.target.getBoundingClientRect()
        //const top = pos.top
        //const bottom = pos.bottom
        const bgColor = e.target.style.backgroundColor
        const selectedColor = 'rgb(255, 255, 0)' // '#ffff00'
        if (bgColor === selectedColor) {
          e.target.style.backgroundColor = "rgb(255, 255, 255)"
        } else {
          e.target.style.backgroundColor = selectedColor
        }
      },
      time () {
        const start = this.turn.start
        const end   = this.turn.end
        const startByMinute = Math.floor(start / 100) * 60 + start % 100
        const endByMinute   = Math.floor(end / 100 ) * 60 + end % 100
        return endByMinute - startByMinute
      },
    },
    filters: {
      hhmmWithDelimiter (hhmm) {
        const hh = Math.floor(hhmm / 100)
        const mm = hhmm % 100
        return `${hh}:${('0' + mm).slice(-2)}`
      }
    },
  }
</script>

<style scoped>
  .cell {
    box-sizing: border-box;
    overflow: hidden;
    font-size: 80%;
    margin-left: 5px;
    margin-right: 5px;
    background-color: white;
  }
</style>
