<template>

  <div class="selectInput">
    <select name="devices" id="devices" class="outline-none mb-4 border-none rounded" value="Synth" v-model="devices">
      <option value="Synth" selected="selected">Synth</option>
      <option value="PolySynth">PolySynth</option>
      <option value="AMSynth">AMSynth</option>
      <option value="MonoSynth">MonoSynth</option>
      <option value="FMSynth">FMSynth</option>
    </select>
  </div>

  <div class="fixed-piano -center h-48">
      <div class="piano w-full h-full flex justify-end items-start relative" v-for="set in setNums" :key="set">
        <div class="whiteNote bg-[snow] w-[50px] h-48 flex flex-col justify-end items-center border rounded border-black csWhite" @click="soundTrigger(noteSymbol[0])" v-for="noteSymbol in notesSymbol[set]" :key="noteSymbol" ref="whiteNote"> <p class="text-black mb-2 uppercase"> {{ noteSymbol[1] }} </p> <p class=""> {{ noteSymbol[0] }}</p>
        </div>
        <div class="blackNote bg-[black] w-[25px] h-32 mr-[37.5px] flex justify-center items-end absolute rounded-bl rounded-br csBlack" @click="soundTrigger(noteSharp[set][4][0])"> <p class="text-[snow] mb-2 font-bold uppercase"> {{ noteSharp[set][4][1] }} </p>
        </div>
        <div class="blackNote bg-[black] w-[25px] h-32 mr-[87.5px] flex justify-center items-end absolute rounded-bl rounded-br csBlack" @click="soundTrigger(noteSharp[set][3][0])"> <p class="text-[snow] mb-2 font-bold uppercase"> {{ noteSharp[set][3][1] }} </p> 
        </div>
        <div class="blackNote bg-[black] w-[25px] h-32 flex justify-center items-end absolute mr-[137.5px] rounded-bl rounded-br csBlack" @click="soundTrigger(noteSharp[set][2][0])"> <p class="text-[snow] mb-2 font-bold uppercase"> {{ noteSharp[set][2][1] }} </p>
        </div>
        <div class="blackNote bg-[black] w-[25px] h-32 flex justify-center items-end absolute mr-[237.5px] rounded-bl rounded-br csBlack" @click="soundTrigger(noteSharp[set][1][0])"> <p class="text-[snow] mb-2 font-bold uppercase"> {{ noteSharp[set][1][1] }} </p>
        </div>
        <div class="blackNote bg-[black] w-[25px] h-32 flex justify-center items-end absolute mr-[287.5px] rounded-bl rounded-br csBlack" @click="soundTrigger(noteSharp[set][0][0])"> <p class="text-[snow] mb-2 font-bold uppercase"> {{ noteSharp[set][0][1] }} </p>
        </div>
    </div>
  </div>

</template>

<script>

import { onUpdated, ref, watch } from '@vue/runtime-core'
import * as Tone from 'tone'

export default ({
  setup() {

    let synth = new Tone.Synth().toDestination()
    const whiteNote = ref(null)
    const setNums = [0, 1, 2]
    let stopSignBlack = true
    let devices = ref("Synth")
    const now = Tone.now()
    let holderBlack = ""
    let stopSign = true
    let holder = ""
    let keyP = ''
    watch(devices, () => {
      if (devices.value == "Synth") { 
        synth = new Tone.Synth().toDestination()
      }
      if (devices.value == "PolySynth") { 
        synth = new Tone.PolySynth().toDestination()
      }
      if (devices.value == "AMSynth") { 
        synth = new Tone.AMSynth().toDestination()
      }
      if (devices.value == "MonoSynth") { 
        synth = new Tone.MonoSynth().toDestination()
      }
      if (devices.value == "FMSynth") { 
        synth = new Tone.FMSynth().toDestination()
      }
    })
    
    const notesSymbol = [
      [['C3', 'q'], ['D3', 'w'], ['E3', 'e'], ['F3', 'r'], ['G3', 't'], ['A3', 'y'], ['B3', 'u']],
      [['C4', 'z'], ['D4', 'x'], ['E4', 'c'], ['F4', 'v'], ['G4', 'b'], ['A4', 'n'], ['B4', 'm']],
      [['C5', ' '], ['D5', ' '], ['E5', ' '], ['F5', ' '], ['G5', ' '], ['A5', ' '], ['B5', ' ']],
      [['C6', ' '], ['D6', ' '], ['E6', ' '], ['F6', ' '], ['G6', ' '], ['A6', ' '], ['B6', ' ']],
    ]
    const noteSharp = [
      [['Db3', '2'], ['Eb3', '3'], ['Gb3', '5'], ['Ab3', '6'], ['Bb3', '7']],
      [['Db4', 's'], ['Eb4', 'd'], ['Gb4', 'g'], ['Ab4', 'h'], ['Bb4', 'j']],
      [['Db5', ' '], ['Eb5', ' '], ['Gb5', ' '], ['Ab5', ' '], ['Bb5', ' ']],
      [['Db6', ' '], ['Eb6', ' '], ['Gb6', ' '], ['Ab6', ' '], ['Bb6', ' ']],
    ]
    const whiteAndBlack = [
        ["q", "w", "e", "r", "t", "y", "u", "z", "x", "c", "v", "b", "n", "m"],
        ['C3', 'D3', 'E3', 'F3', 'G3', 'A3',' B3',' C4',' D4', 'E4', 'F4',' G4', 'A4', 'B4'],
        ["2", "3", "5", "6", "7", "s", "d", "g", "h", "j"],
        ['Db3', 'Eb3', 'Gb3', 'Ab3', 'Bb3', 'Db4', 'Eb4', 'Gb4', 'Ab4', 'Bb4']
      ]

    const keyEvent = window.addEventListener('keypress', (event) => {
      keyP = event.key
      for (let i = 0; i < whiteAndBlack[0].length; i++) {
        if (keyP == whiteAndBlack[0][i]) {
          if (stopSign) {
            soundTrigger(whiteAndBlack[1][i])
            for (let j = 0; j < whiteNote.value.length; j++) {
              if (keyP == (whiteNote.value)[j].children[0].innerHTML) {
                whiteNote.value[j].classList.toggle('csWhiteClicked')
                holder = whiteNote.value[j]
                stopSign = false
              }
            }
          }
        }
      }
      for (let i = 0; i < whiteAndBlack[2].length; i++) {
        if (keyP == whiteAndBlack[2][i]) {
          if (stopSignBlack) {
            soundTrigger(whiteAndBlack[3][i])
            var allBlackNote = document.getElementsByClassName("blackNote")
            for (let j = 0; j < allBlackNote.length; j++) {
              if (keyP == (allBlackNote)[j].children[0].innerHTML) {
                allBlackNote[j].classList.toggle('csBlackClicked')
                holderBlack = allBlackNote[j]
                stopSignBlack = false
              }
            }
          }
        }
      }
    })
    const finishedEevent = window.addEventListener("keyup", (event) => {
      console.log(event)
      if (holder) {
        holder.classList.toggle('csWhiteClicked')
        holder = ''
        stopSign = true
      }
      else {
        holderBlack.classList.toggle('csBlackClicked')
        holderBlack = ''
        stopSignBlack = true
      }
    })
    function soundTrigger(note, time="8n") {
      synth.triggerAttackRelease(note, time)
      console.log(note, now)
    }
    return { setNums, notesSymbol, noteSharp, soundTrigger, whiteNote, devices }
  },
})
</script>


<style>

* {
  font-family: 'Sofia Sans Extra Condensed', sans-serif;
}

.csBlack {
  box-shadow: rgba(240, 46, 170, 0.4) 0px 5px, rgba(240, 46, 170, 0.3) 0px 10px, rgba(240, 46, 170, 0.2) 0px 15px, rgba(240, 46, 170, 0.1) 0px 20px, rgba(240, 46, 170, 0.05) 0px 25px;

}
.csBlack {
  box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 2px 0px;
  }

.csBlack:active {
  box-shadow: rgba(240, 46, 170, 0.4) 0px 5px, rgba(240, 46, 170, 0.3) 0px 10px, rgba(240, 46, 170, 0.2) 0px 15px, rgba(240, 46, 170, 0.1) 0px 20px, rgba(240, 46, 170, 0.05) 0px 25px;
}

.csBlackClicked {
  box-shadow: rgba(240, 46, 170, 0.4) 0px 5px, rgba(240, 46, 170, 0.3) 0px 10px, rgba(240, 46, 170, 0.2) 0px 15px, rgba(240, 46, 170, 0.1) 0px 20px, rgba(240, 46, 170, 0.05) 0px 25px;
}

.csWhite {
  box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px, rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
}

.csWhite:active {
  box-shadow: rgb(204, 219, 232) 3px 3px 6px 0px inset, rgba(255, 255, 255, 0.5) -3px -3px 6px 1px inset;
}

.csWhiteClicked { 
  box-shadow: rgb(204, 219, 232) 3px 3px 6px 0px inset, rgba(255, 255, 255, 0.5) -3px -3px 6px 1px inset;
}

</style>
