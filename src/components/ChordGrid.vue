<template>
  <div id="chordGrids" ref="chordGrids"
       :style="{ 'grid-template-rows' : `repeat(${rows}, minmax(60px, 1fr))` }"
       @mouseup="onmouseup"
  >
    <ChordSymbol v-for="(chord, index) in chords"
                 :key="chord.uuid"
                 :uuid="chord.uuid"
                 :chord-string="chord.chordString"
                 :begin-position="chord.beginPosition"
                 :style="calcGridPosition(chord)"
                 :duration="chord.duration"
                 :tabindex="index"
                 :show_remove_handler="false"
                 @remove_chord="removeChord"
    />
    <div id="caret"
         ref="caret"
         :style="{
        'top': caret.position[0],
        'left': caret.position[1],
        'display': caret.visible ? 'block' : 'none',
        'height': caretHeight()
      }"
    ></div>
  </div>
</template>

<script lang="ts">


import {v4} from 'uuid'
import {defineComponent} from 'vue'
import ChordSymbol from './ChordSymbol.vue'

function randomChoice(arr: Array<any>) {
  return arr[Math.floor(Math.random() * arr.length)]
}

export interface Chord {
  uuid?: string;
  chordString: string;
  beginPosition: Array<number>;
  duration?: number;
}

function DefaultChords(): Array<Chord> {
  const N = 16
  const arr: Array<Chord> = new Array(N)
  // console.log('from default', this)
  for (let i = 0; i < N; i++)
    arr.push({
      uuid: v4(),
      chordString: randomChoice("A B C D E F G".split(' ')) +
          (Math.random() > 0.5 ? randomChoice(['b', '#']) : '') +
          randomChoice(['', 'm7', '7', 'Maj7', '13', 'dim7', '7sus4']),
      beginPosition: [i, 0],
      duration: 4
    })
  return arr
}

const ChordGrid = defineComponent({
  name: "ChordGrid",
  components: {
    ChordSymbol
  },
  data() {
    return {
      chords: DefaultChords(),
      timeSignature: 4,
      caret: {
        coordinate: [0, 0], // row, column
        position: ['0px', '0px'],
        visible: true
      },
      chordsPerRow: 4
    }
  },
  computed: {
    rows(): number {
      const maxMeasure = Math.max(...this.chords.map(chord => chord.beginPosition[0]))
      const rows = Math.floor(maxMeasure / this.chordsPerRow) + 1
      return Math.max(rows, 4)
    },
    columns(): number {
      return this.timeSignature * this.chordsPerRow
    }
  },
  created() {
    window.addEventListener('resize', this.updateCaret)
  },
  methods: {
    calcUnitSize() { // in unit of px
      const grid = document.querySelector('chordGrids')
      if (grid == null)
        return -1
      let w = grid.offsetWidth
      let h = grid.offsetHeight
      return {
        width: w / this.columns,
        height: h / this.rows
      }
    },
    updateCaret() {
      // console.log('updating caret')
      // eslint-disable-next-line @typescript-eslint/no-this-alias
      const self = this
      const size = this.calcUnitSize()
      const grid = this.$refs.chordGrids
      const left = grid.getAttribute('left')
      const top = grid.getAttribute('top')
      this.caret.position = [
        self.caret.coord[0] * size.height + top + 'px',
        self.caret.coord[1] * size.width + left + 'px'
      ]
      // console.log('new_position', this.caret.position)
      setTimeout(
          function () {
            // let caret = document.getElementById('caret')
            const caret = document.querySelector('caret')
            caret.style.setProperty('height', self.caretHeight())
            caret.style.setProperty('top', self.caret.position[0])
            caret.style.setProperty('left', self.caret.position[1])
          },
          300
      )
    },
    calcGridPosition: function (chord: Chord) {
      const beginPosition = chord.begin_position
      const duration = chord.duration

      const rowStart = 1 + Math.floor(beginPosition[0] / 4);
      const columnStart = 1 + beginPosition[0] % 4 * this.timeSignature + beginPosition[1]
      return {
        'grid-row-start': rowStart,
        'grid-column': columnStart + ' / ' + (columnStart + duration)
      }
    },
    onmouseup: function (e) {
      const grid = document. querySelector('#chordGrids')
      const x0 = grid.offsetLeft
      const y0 = grid.offsetTop
      const w = grid.offsetWidth
      const h = grid.offsetHeight
      const px = e.pageX
      const py = e.pageY
      const x = px - x0, y = py - y0
      const rowHeight = h / this.rows
      const columnWidth = w / this.columns
      const j = Math.floor(x / columnWidth)
      const i = Math.floor(y / rowHeight)
      // console.log(x0, y0, w, h, px, py, i, j)

      // let caret = document.getElementById('caret')
      if (i < 0 || j < 0 || j >= this.columns || i >= this.rows) {
        this.caret.visible = false
      } else {
        this.caret.visible = true
        this.caret.coord = [i, j]
        this.caret.position = [i * rowHeight + 'px', j * columnWidth + 'px']

      }
    },
    removeChord(uuid) {
      // console.log(uuid)
      // console.log(this.chords.map(c => c.uuid))
      const index = this.chords.findIndex(c => c.uuid == uuid)
      // console.log('index', index)
      if (index > -1) {
        console.log('delete', index)
        this.chords.splice(index, 1)
      }
      this.updateCaret()
    },
    caretHeight: function () {
      let h
      try {
        h = document.getElementById("chordGrids").offsetHeight
      } catch (e) {
        // console.log(e)
        h = 100
      }
      // console.log('height', h / this.rows)
      return h / this.rows + 'px'
    }
  }
})

export default ChordGrid
</script>

<style scoped>

</style>