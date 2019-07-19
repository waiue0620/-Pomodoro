<template>
  <div class="pb-5">
    <section class="mb-4">
      <div class="d-flex h4 px-3 py-2 whiteOpacity20 mb-3">work</div>
      <div class="row">
        <div class="col-4 d-flex mb-3 cursor-pointer" v-for="(item, index) in ringtones" :key="index" @click="changeRingtones(item, 'Work')">
          <i class="material-icons mr-1" v-if="tempSelectWork != item.name">radio_button_unchecked</i>
          <i class="material-icons mr-1 text-primary" v-if="tempSelectWork == item.name">radio_button_checked</i>
          <p class="m-0">{{ item.name }}</p>
        </div>
      </div>
    </section>
    <section>
      <div class="d-flex h4 px-3 py-2 whiteOpacity20 mb-3">break</div>
      <div class="row">
        <div class="col-4 d-flex mb-3 cursor-pointer" v-for="(item, index) in ringtones" :key="index"  @click="changeRingtones(item, 'Dreak')">
          <i class="material-icons mr-1" v-if="tempSelectDreak != item.name">radio_button_unchecked</i>
          <i class="material-icons mr-1 text-primary" v-if="tempSelectDreak == item.name">radio_button_checked</i>
          <p class="m-0">{{ item.name }}</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>

export default {
  name: 'home',
  props: {
    selectWork: {
      type: String,
      default: function () {
        return ''
      }
    },
    selectDreak: {
      type: String,
      default: function () {
        return ''
      }
    }
  },
  data () {
    return {
      ringtones: [
        {
          name: 'none',
          path: ''
        },
        {
          name: 'Default',
          path: ''
        },
        {
          name: 'alarm',
          path: ''
        },
        {
          name: 'alert',
          path: ''
        },
        {
          name: 'beep',
          path: ''
        },
        {
          name: 'bell',
          path: ''
        },
        {
          name: 'bird',
          path: ''
        },
        {
          name: 'bugle',
          path: ''
        },
        {
          name: 'digital',
          path: ''
        },
        {
          name: 'drop',
          path: ''
        },
        {
          name: 'horn',
          path: ''
        },
        {
          name: 'music',
          path: ''
        },
        {
          name: 'ring',
          path: ''
        },
        {
          name: 'warning',
          path: ''
        },
        {
          name: 'whistle',
          path: ''
        }
      ],
      tempSelectWork: this.selectWork,
      tempSelectDreak: this.selectDreak,
      snd: new Audio()
    }
  },
  methods: {
    changeRingtones (item, status) {
      const vm = this
      status === 'Work' ? vm.tempSelectWork = item.name : vm.tempSelectDreak = item.name
      vm.snd.pause()
      if (item.name === 'none') {
        vm.snd = new Audio()
      } else {
        status === 'Work' ? vm.snd = new Audio(`ringtones/${vm.tempSelectWork}.mp3`) : vm.snd = new Audio(`ringtones/${vm.tempSelectDreak}.mp3`)
      }
      vm.snd.play()
      vm.$emit('changeRingtones', vm.tempSelectWork, vm.tempSelectDreak)
    }
  }
}
</script>
