<template>
  <div class="symbol" v-on:keyup.delete="removeChordEvent">
    <div class="component root">
      {{ root ? root[0] : '' }}<sup class="accidental">{{ getAccidental(root) }}</sup>
    </div>
    <div class="component chord_type">
      {{ chordType }}
    </div>
    <div class="component extension" v-if=hasExtension>
    </div>
    <div class="component slash" v-if=hasBass>
      /
    </div>
    <div class="component bass" v-if=hasBass>
      {{ bass[0] }}<sup class="accidental">{{ getAccidental(bass) }}</sup>
    </div>
    <button class="remove" v-if="showRemoveHandler"></button>
  </div>
</template>

<script lang="ts">

import {defineComponent} from 'vue'

interface MyData {
  bass: string | null
  root: string | null
  chordType: string | null
}

const ChordSymbol = defineComponent({
  name: "ChordSymbol",
  props: {
    chordString: String,
    beginPosition: Array,
    duration: {
      default: 4
    },
    showRemoveHandler: {
      type: Boolean,
      default: true
    },
    uuid: String
  },
  data: function () {
    let match = this.chordString.match(
        /(?<root>[a-gA-G](b|#|x|bb)?)(?<type>.*)(\/(?<bass>[a-gA-G](b|#|x|bb)?))?/
        // TODO: Deal with cases like 'Bbb dim7'
    )
    let root: string | null, chordType: string | null, bass: string | null
    if (!match) {
      console.warn(this.chordString, 'isn\'t recognized as a valid chord!')
      root = chordType = bass = null
    } else {
      root = match.groups['root']
      chordType = match.groups['type']
      bass = match.groups['bass']
    }
    return {
      root: root,
      chordType: chordType,
      bass: bass
    } as MyData
  },
  computed: {
    hasExtension: function () {
      return false
    },
    hasBass: function () {
      return this.bass != null
    }
  },
  methods: {
    getAccidental(noteStr: string): string {
      for (const accidental of "# b x bb".split(' ')) {
        if (noteStr.match(accidental))
          return accidental
      }
      return ""
    },
    removeChordEvent() {
      // console.log('from symbol', event)
      this.$emit("remove_chord", this.uuid)
    }
  }
})

export default ChordSymbol

</script>

<style scoped>

</style>