<template>
  <div class="container">
    <div class="output">
      <input type="button" v-on:click="output" value="Output of Image">
    </div>
    <div class="choice">
      <input type="radio" id="one" value="day1" v-model="checkedDay">
      <label for="one">8/26</label>
      <input type="radio" id="two" value="day2" v-model="checkedDay">
      <label for="two">8/27</label>
    </div>
    <p>Current Showing: {{ checkedDay }}</p>
    <div class="timetable" id="day1" v-if="checkedDay === 'day1'">
      <table-column
        v-for="stage in stagesOfFirst"
        :name="stage.stage"
        :color="stage.color"
        :turns="stage.turns"
        :key="stage.id">
      </table-column>
    </div>
    <div class="timetable" id="day2" v-else>
      <table-column
        v-for="stage in stagesOfSecond"
        :name="stage.stage"
        :color="stage.color"
        :turns="stage.turns"
        :key="stage.id">
      </table-column>
    </div>
  </div>
</template>

<style>
  .timetable {
    display: table;
    table-layout: fixed;
    width: 100%;
    height: 3300px;
    margin: 0 auto;
  }
  .timetable > div {
    display: table-cell;
  }
</style>

<script>
  import TableColumn from './TableColumn'
  import DomToImage from 'dom-to-image'
  import JSON1 from '../../tt-1.json' // TODO: delete after you can read remote json file
  import JSON2 from '../../tt-2.json' // TODO: delete after you can read remote json file
  // const TTOfDay1URL = 'https://leadi.jp/json/14680.json'
  // const TTOfDay2URL = 'https://leadi.jp/json/14681.json'

  export default {
    name: 'table-container',
    components: {
      TableColumn,
    },
    data () {
      return {
        checkedDay: "day1",
        stagesOfFirst: [],
        stagesOfSecond: []
      }
    },
    created () {
      this.stagesOfFirst = JSON1.data.timetables
      this.stagesOfSecond = JSON2.data.timetables
    },
    methods: {
      output () {
        const targetNode = document.getElementById(this.checkedDay)
        DomToImage.toBlob(targetNode)
          .then((blob) => {
            const fileURL = URL.createObjectURL(blob)
            window.open(fileURL)
          })
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
