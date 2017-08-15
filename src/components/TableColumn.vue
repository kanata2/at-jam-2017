<template>
  <div class="stage" :style="{ 'background-color': color }">
    <div class="header">
      {{ name | cutSuffix }}
    </div>
    <table-cell
      class="turns"
      v-for="turn in nturns"
      v-on:click="getPos()"
      v-bind:turn="turn"
      v-bind:color="color"
      :key="turn.id">
      {{ turn.id }}
    </table-cell>
  </div>
</template>

<script>
  import TableCell from './TableCell'

  export default {
    name: 'table-column',
    components: {
      TableCell,
    },
    props: ['name', 'color', 'turns'],
    data () {
      return {
        nturns: this.turnsWithOffset(),
      }
    },
    filters:  {
      cutSuffix (val) {
        const re = /ステージ$/
        return val.replace(re, '')
      }
    },
    methods: {
      turnsWithOffset () {
          return this.turns.map((turn, idx, ary) => {
            let prevEnd
            if (idx === 0) {
              prevEnd = 900
            } else {
              const prev = ary[idx-1]
              prevEnd = prev.end
            }
            const start = turn.start
            const prevEndByMinute = Math.floor(prevEnd / 100) * 60 + prevEnd % 100
            const startByMinute = Math.floor(start / 100) * 60 + start % 100
            const offset = (startByMinute - prevEndByMinute) * 5
            return Object.assign(turn, { offset: offset })
          })
      },
      getPos (e) {
        console.log(e)
      },
    },
  }
</script>

<style scoped>
  .header {
    height: 25px;
  }
</style>
