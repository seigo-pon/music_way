<template>
  <div class="music">
    <table align="center">
      <tr>
        <td>かえるのうた</td>
        <td>:</td>
        <td><button v-bind:disabled="!enablePlay['frog']" @click="play('frog')">{{ playState["frog"] }}</button></td>
      </tr>
      <tr>
        <td>オリジナル</td>
        <td>:</td>
        <td><button v-bind:disabled="!enablePlay['original']" @click="play('original')">{{ playState["original"] }}</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
let ongaqs = {}

export default {
  name: 'Music',
  data () {
    return {
      enablePlay: {
        "frog": false,
        "original": false
      },
      playState: {
        "frog": "Ready",
        "original": "Ready"
      }
    }
  },
  methods: {
    createFrogSong () {
      const ongaq = new window.Ongaq({
        api_key: process.env.VUE_APP_ONGAQ_API_KEY,
        volume: 80,
        bpm: 60,
        onReady: () => {
          this.enablePlay["frog"] = true
          this.playState["frog"] = "Play"
        }
      })

      const myPiano = new window.Part({
        sound: "my_piano",
        measure: 4
      })
      myPiano.add(new window.Filter({
        key: (beat) => {
          switch (beat) {
          case 0: return ["C2"]
          case 2: return ["D2"]
          case 4: return ["E2"]
          case 6: return ["F2"]
          case 8: return ["E2"]
          case 10: return ["D2"]
          case 12: return ["C2"]
          }
        },
        length: 2,
        active: (beat, measure) => {
          return (measure === 0 && ((beat < 14) && (beat % 2 === 0)))
        }
      }))
      myPiano.add(new window.Filter({
        key: (beat) => {
          switch (beat) {
          case 0: return ["E2"]
          case 2: return ["F2"]
          case 4: return ["G2"]
          case 6: return ["A2"]
          case 8: return ["G2"]
          case 10: return ["F2"]
          case 12: return ["E2"]
          }
        },
        length: 2,
        active: (beat, measure) => {
          return (measure === 1 && ((beat < 14) && (beat % 2 === 0)))
        }
      }))
      myPiano.add(new window.Filter({
        key: ["C2"],
        length: 3,
        active: (beat, measure) => {
          return (measure === 2 && (beat % 4 === 0))
        }
      }))
      myPiano.add(new window.Filter({
        key: (beat) => {
            switch (beat) {
            case 0: return ["C2"]
            case 1: return ["C2"]
            case 2: return ["D2"]
            case 3: return ["D2"]
            case 4: return ["E2"]
            case 5: return ["E2"]
            case 6: return ["F2"]
            case 7: return ["F2"]
            case 8: return ["E2"]
            case 10: return ["D2"]
            case 12: return ["C2"]
            }
        },
        length: (beat) => {
            if (beat < 8) {
                return 1
            } else {
                return 2
            }
        },
        active: (beat, measure) => {
          return (measure === 3 && ((beat < 14) && ((beat < 8) || (beat >= 8 && beat % 2 === 0))))
        }
      }))

      ongaq.add(myPiano)

      return ongaq
    },
    createOriginalSong () {
      const ongaq = new window.Ongaq({
        api_key: process.env.VUE_APP_ONGAQ_API_KEY,
        volume: 80,
        bpm: 60,
        onReady: () => {
          this.enablePlay["original"] = true
          this.playState["original"] = "Play"
        }
      })

      const myDrum = new window.Part({
        sound: "small_cube_drums",
        measure: 4
      })
      myDrum.add(new window.Filter({
        key: "kick",
        active: n => (n % 8 === 0 || n % 8 === 4)
      }))
      myDrum.add(new window.Filter({
        key: "snare",
        active: n => (n % 8 === 1 || n % 8 === 5)
      }))
      myDrum.add(new window.Filter({
        key: "hihat",
        active: (beat, measure) => {
          return (beat === 15) || (measure === 3 && beat === 14)
        }
      }))

      const myKeyboard = new window.Part({
        sound: "plain_keyboard",
        measure: 8
      })
      myKeyboard.add(new window.Filter({
        key: ["C3", "E3", "G3"],
        length: (beat, measure) => {
          if (measure === 6) {
            return 16
          } else {
            return 32
          }
        },
        active: (beat, measure) => {
          return (measure !== 7) && ((measure % 2 === 0) && (beat === 0))
        }
      }))
      myKeyboard.add(new window.Filter({
        key: (beat) => {
          switch (beat) {
            case 0: return ["C3", "E3", "G3"]
            case 8: return ["E3", "G3", "B3"]
          }
        },
        length: 8,
        active: (beat, measure) => {
          return (measure === 7) && (beat === 0 || beat === 8)
        }
      }))

      const myKeyboard2 = new window.Part({
        sound: "plain_keyboard",
        measure: 16
      })
      myKeyboard2.add(new window.Filter({
        key: ["C2", "E2", "G2"],
        length: (beat) => {
          if (beat === 3) {
            return 30
          } else {
            return 2
          }
        },
        active: (beat, measure) => {
          return (measure >= 8) && ((measure % 2 === 0) && (beat === 0 || beat === 2 || beat === 3))
        }
      }))
      myKeyboard2.add(new window.Filter({
        key: (beat) => {
          switch (beat) {
            case 0: return ["C2", "E2", "G2"]
            case 2: return ["C2", "E2", "G2"]
            case 3: return ["C2", "E2", "G2"]
            case 8: return ["E2", "G2", "B2"]
            case 9: return ["E2", "G2", "B2"]
            case 10: return ["E2", "G2", "B2"]
          }
        },
        length: (beat) => {
          if (beat === 3 || beat === 10) {
            return 12
          } else {
            return 2
          }
        },
        active: (beat, measure) => {
          return (measure === 15) && (beat === 0 || beat === 2 || beat === 3 || beat === 8 || beat === 9 || beat === 10)
        }
      }))
      myKeyboard2.add(new window.Filter({
        type: "pan",
        x: -45
      }))

      const myBase = new window.Part({
        sound: "mono_bass",
        measure: 4
      })
      myBase.add(new window.Filter({
          key: ["G1"],
          length: 2,
          active: n => (n === 0 || n === 2 || n === 4)
      }))
      myBase.add(new window.Filter({
          key: ["E1"],
          length: 2,
          active: (beat, measure) => {
              return (measure === 3) && (beat === 6 || beat === 7)
          }
      }))
      myBase.add(new window.Filter({
        type: "pan",
        x: 45
      }))

      ongaq.add(myDrum)
      ongaq.add(myKeyboard)
      ongaq.add(myKeyboard2)
      ongaq.add(myBase)

      return ongaq
    },
    play (song) {
      if (ongaqs[song].params.isPlaying) {
        ongaqs[song].pause()
        this.playState[song] = "Play"
      } else {
        ongaqs[song].start()
        this.playState[song] = "Pause"
      }
    }
  },
  mounted () {
    ongaqs["frog"] = this.createFrogSong()
    ongaqs["original"] = this.createOriginalSong()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
  padding: 1px 10px;
  font-size: 18px;
  color: #555555;
  border: 2px solid #dddddd;
  cursor: pointer;
  outline: 0;
}
</style>
