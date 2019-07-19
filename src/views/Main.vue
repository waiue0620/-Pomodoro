<template>
  <div>
    <div class="d-flex" :class="{ 'rest':reciprocal.rest }">
      <div class="d-flex justify-content-center align-items-center bg-primaryLight leftBlock position-relative">
        <section class="todo align-self-start mt-5">
          <div class="pb-5">
            <input class="input text-primary border-0 font-italic pl-3" type="text" placeholder="ADD A NEW MISSION…" v-model.trim="newMission" @keydown.enter="addNewMission(newMission)">
            <i class="material-icons inputIcons bg-white text-primary cursor-pointer" @click="addNewMission(newMission)">add</i>
          </div>
          <div class="d-flex mt-5" v-if="!getNotDoneMission.length == 0">
            <div class="done mr-3 cursor-pointer" @click="doneMission(getNotDoneMission[0])"><!-- <i class="material-icons doneIcons">done</i> --></div>
            <div v-for="(item, index) in getNotDoneMission.slice(0, 1)" :key="index">
              <div class="h4">{{ item.title }}</div>
              <div class="d-flex">
                <div class="done done-sm bg-secondary mr-2" v-for="(item, index) in getNotDoneMission[0].tomato" :key="index"></div>
                <div class="notDoneTomato mr-2" v-if="!reciprocal.rest"></div>
              </div>
            </div>
          </div>
          <div class="time text-primary" v-if="!getNotDoneMission.length == 0">
            <!-- <p>30:00</p> -->
            <!-- <p>{{ String(reciprocal[0].reciprocalTime) }}</p> -->
            <p>{{ getRemainTime }}</p>
          </div>
          <ul class="p-0">
            <li class="d-flex missionList pb-2 mb-2" v-for="(item, index) in getNotDoneMission.slice(1,4)" :key="index">
              <div class="done done-md mr-1 cursor-pointer"  @click="doneMission(item)"><!-- <i class="material-icons doneIcons">done</i> --></div>
              <p class="m-0 mr-auto">{{ item.title }}</p>
              <i class="material-icons cursor-pointer" @click="changMission(item,index)">play_circle_outline</i>
            </li>
            <!-- <li class="d-flex missionList pb-2 mb-2">
              <div class="done done-md mr-1"></div>
              <p class="m-0 mr-auto">the second thing to do today</p>
              <i class="material-icons">play_circle_outline</i>
            </li> -->
            <!-- <li class="d-flex missionList pb-2 mb-2">
              <div class="done done-md mr-1"></div>
              <p class="m-0 mr-auto">the third thing to do today</p>
              <i class="material-icons">play_circle_outline</i>
            </li>
            <li class="d-flex missionList pb-2 mb-2">
              <div class="done done-md mr-1"></div>
              <p class="m-0 mr-auto">the forth thing to do today</p>
              <i class="material-icons">play_circle_outline</i>
            </li> -->
          </ul>
          <p class="text-primary text-right cursor-pointer" v-if="getNotDoneMission.length > 4" data-toggle="modal" data-target="#menuModal" @click="changeView('mission')">MORE</p>
        </section>
        <section class="position-absolute playMenu">
          <div class="OuterCircle border-primary rounded-circle d-flex justify-content-center align-items-center position-relative">
            <svg class="svg position-absolute" width="540" height="540" v-if="!reciprocal.pause">
              <circle class="progress-ring__circle" stroke="#FF4384" stroke-width="15" fill="transparent" r="260" cx="270" cy="270"
              :stroke-dasharray="getStrokeDasharray(260)" :stroke-dashoffset="getStrokeDashoffset(260)"/>
            </svg>
            <div class="InnerCircle bg-primary rounded-circle d-flex justify-content-center align-items-center" :class="{'start' : !reciprocal.pause}">
              <i class="material-icons playCircle md-48 cursor-pointer" @click="startReciprocal" v-if="reciprocal.pause">play_circle_filled</i>
              <i class="material-icons playCircle md-48 cursor-pointer text-primary" @click="pauseReciprocal" v-if="!reciprocal.pause">pause_circle_filled</i>
              <i class="material-icons stop cursor-pointer text-white" :class="{ 'text-primary' : !reciprocal.pause}" @click="invalidReciprocal">stop</i>
            </div>

            <div class="d-flex flex-column text-white position-absolute menu">
              <div class="d-flex flex-column">
                <i class="material-icons menu-icons mb-5 cursor-pointer" data-toggle="modal" data-target="#menuModal" @click="changeView('mission')">list</i>
                <i class="material-icons menu-icons mb-5 cursor-pointer" data-toggle="modal" data-target="#menuModal" @click="changeView('analytics')">insert_chart</i>
                <i class="material-icons menu-icons cursor-pointer" data-toggle="modal" data-target="#menuModal" @click="changeView('ringtones')">library_music</i>
              </div>
              <div class="textColumn h4"  @click="reciprocalTime">POMODORO</div>
            </div>

          </div>
        </section>
      </div>
      <div class="bg-secondary rightBlock d-flex justify-content-end">
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade p-0" id="menuModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="container">
            <div class="row justify-content-between h-100">
              <div class="col-4 mt-5 position-relative">
                <ul>
                  <li class="d-flex menu-text cursor-pointer" :class="{'active':view==='mission'}" @click="changeView('mission')">
                    <i class="material-icons menu-icons cursor-pointer align-self-center mr-2" data-toggle="modal" data-target="#menuModal">list</i>
                    <p class="m-0">to-do list</p>
                  </li>
                  <li class="d-flex menu-text cursor-pointer" :class="{'active':view==='analytics'}" @click="changeView('analytics')">
                    <i class="material-icons menu-icons cursor-pointer align-self-center mr-2" data-toggle="modal" data-target="#menuModal">insert_chart</i>
                    <p class="m-0">analytics</p>
                  </li>
                  <li class="d-flex menu-text cursor-pointer" :class="{'active':view==='ringtones'}" @click="changeView('ringtones')">
                    <i class="material-icons menu-icons cursor-pointer align-self-center mr-2" data-toggle="modal" data-target="#menuModal">library_music</i>
                    <p class="m-0">ringtones</p>
                  </li>
                </ul>
                <div class="position-absolute menuCircleDisp" :class="{ 'rest':reciprocal.rest }">
                  <div class="menuCircle">
                    <div class="text-primary d-flex flex-column align-items-center pt-5">
                      <p class="m-0">{{ getRemainTime }}</p>
                      <div class="h6 text-secondary">{{ getNotDoneMission[0].title }}</div>
                      <div class="position-absolute menuplayCircleDisp cursor-pointer">
                        <div class="menuplayCircle d-flex justify-content-center align-items-center">
                          <div class="menuouterCircle d-flex justify-content-center align-items-center">
                            <div class="bg-white rounded-circle menuinnerCircle d-flex justify-content-center align-items-center">
                              <i class="material-icons" @click="startReciprocal" v-if="reciprocal.pause">play_circle_filled</i>
                              <i class="material-icons" @click="pauseReciprocal" v-if="!reciprocal.pause">pause_circle_filled</i>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-5 mt-5">
                <component :is="view" :not-done-mission='getNotDoneMission' :done-mission='getDoneMission' :mission='mission' :selectWork='selectWork' :selectDreak='selectDreak' @addNewMission='addNewMission'
                @sendToParentMission='getComponentsMission' @doneMission='doneMission' @changMission='changMission' @changeRingtones='changeRingtones'></component>
              </div>
              <div class="col-1 d-flex flex-column justify-content-between align-items-end my-5">
                <div class="">
                  <i class="fs48 material-icons cursor-pointer" @click="offMenuModal">clear</i>
                </div>
                <div class="h4 textColumn">POMODORO</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import $ from 'jquery'
import Mission from '@/components/Mission'
import Analytics from '@/components/Analytics'
import Ringtones from '@/components/Ringtones'

export default {
  name: 'home',
  components: {
    Mission,
    Analytics,
    Ringtones
  },
  data () {
    return {
      mission: [
        {
          title: ''
        }
      ],
      view: '',
      reciprocal: {
        reciprocalTime: 1500,
        pause: true,
        rest: false
      },
      newMission: '',
      StrokeDasharrayTime: 1500,
      selectWork: '',
      selectDreak: '',
      snd: new Audio()
    }
  },
  methods: {
    getMissioncts () {
      const vm = this
      const url = `${process.env.VUE_APP_APIPATH}/mission`
      vm.$http.get(url).then(response => {
        console.log(response.data)
        vm.mission = response.data
        // let temp = [{
        //   id: vm.mission[0].id,
        //   reciprocalTime: 6,
        //   pause: false,
        //   rest: false
        // }]
        // vm.reciprocal = temp
      })
    },
    offMenuModal () {
      $('#menuModal').modal('hide')
    },
    changeView (viewName) {
      const vm = this
      vm.view = viewName
    },
    startReciprocal () {
      const vm = this
      // let temp = {
      //   id: vm.mission[0].id,
      //   reciprocalTime: 6,
      //   pause: false,
      //   rest: false
      // }
      // vm.reciprocal.push(temp)
      // const set = new Set()
      // const result = vm.reciprocal.filter(item => !set.has(item.id) ? set.add(item.id) : false)
      // vm.reciprocal = result
      if (vm.getNotDoneMission === '') { return }
      vm.reciprocal.id = vm.mission[0].id
      vm.reciprocal.pause = false
      setTimeout(function () {
        vm.reciprocalTime()
      }, 1000)
      console.log(vm.reciprocal)
    },
    invalidReciprocal () {
      const vm = this
      vm.reciprocal.reciprocalTime = 1500
      vm.StrokeDasharrayTime = 1500
      vm.reciprocal.pause = true
      vm.reciprocal.rest = false
    },
    pauseReciprocal () {
      const vm = this
      vm.reciprocal.pause = true
    },
    changMission (item, index) {
      const vm = this
      vm.reciprocal.id = item.id
      // let tempMission = vm.mission[0]
      let tempMission = vm.getNotDoneMission[0]
      // vm.$set(vm.mission, index + 1, tempMission)
      // vm.$set(vm.mission, 0, item)
      // function isLargeNumber(element) {
      //   return element.id == item.id
      // }
      // function isLargeNumber2(element) {
      //   return element.id == vm.getNotDoneMission[0].id
      // }
      // let index1 = vm.mission.findIndex(isLargeNumber)
      // let index2 = vm.mission.findIndex(isLargeNumber2)
      // vm.$set(vm.mission, index1, tempMission)
      // vm.$set(vm.mission, index2, item)
      vm.$set(vm.mission, vm.getMissionIndex(item), tempMission)
      vm.$set(vm.mission, vm.getMissionIndex(vm.getNotDoneMission[0]), item)
    },
    reciprocalTime () {
      const vm = this
      if (vm.reciprocal.reciprocalTime === 0) {
        vm.reciprocal.pause = true
        vm.reciprocal.reciprocalTime = 300
        vm.StrokeDasharrayTime = 300
        vm.snd.pause()
        if (!vm.reciprocal.rest) {
          vm.snd = new Audio(`ringtones/${vm.selectWork}.mp3`)
          vm.snd.play()
        } else {
          vm.snd = new Audio(`ringtones/${vm.selectDreak}.mp3`)
          vm.snd.play()
        }
        vm.reciprocal.rest = !vm.reciprocal.rest
        if (vm.reciprocal.rest) { vm.mission[vm.getMissionIndex(vm.getNotDoneMission[0])].tomato++ }
        vm.patchMission(vm.getNotDoneMission[0])
      }
      if (!vm.reciprocal.pause) {
        vm.reciprocal.reciprocalTime--
        console.log(vm.reciprocal.reciprocalTime)
        setTimeout(function () {
          vm.reciprocalTime()
        }, 1000)
      }
      /* vm.reciprocal.forEach(item => {
        if (!item.pause) {
          item.reciprocalTime--
          console.log(item.reciprocalTime)
        }
        if (item.reciprocalTime === 0) {
          item.pause = true
          item.reciprocalTime = 6
          item.rest = true
        }
      }) */
    },
    getStrokeDasharray (r) {
      return r * 2 * Math.PI
    },
    getStrokeDashoffset (r) {
      return this.getStrokeDasharray(r) - this.reciprocal.reciprocalTime / this.StrokeDasharrayTime * this.getStrokeDasharray(r)
    },
    doneMission (items) {
      const vm = this
      console.log(items)
      function isLargeNumber (element) {
        return element.id === items.id
      }
      let index = vm.mission.findIndex(isLargeNumber)
      vm.mission[index].done = !vm.mission[index].done
      vm.patchMission(vm.mission[index])
    },
    patchMission (item) {
      const vm = this
      const url = `${process.env.VUE_APP_APIPATH}/mission/${item.id}`
      // function isLargeNumber (element) {
      //   return element.id === item.id
      // }
      // let index = vm.mission.findIndex(isLargeNumber)
      let index = vm.getMissionIndex(item)
      vm.$http.patch(url, vm.mission[index]).then(response => {
        console.log(response)
      })
    },
    addNewMission (newMission) {
      const vm = this
      console.log(newMission)
      if (vm.newMission === '' && newMission === '') { return }
      let tempNewMission = {
        title: newMission,
        done: false,
        date: 1561910400,
        tomato: 0,
        id: Math.floor(new Date() / 1000)
      }
      vm.mission.push(tempNewMission)
      const url = `${process.env.VUE_APP_APIPATH}/mission`
      vm.$http.post(url, tempNewMission).then(response => {
        // console.log(response)
      })
      vm.newMission = ''
      console.log(vm.mission)
    },
    getMissionIndex (item) {
      const vm = this
      function isLargeNumber (element) {
        return element.id === item.id
      }
      return vm.mission.findIndex(isLargeNumber)
    },
    getComponentsMission (value) {
      this.mission = value
    },
    getRingtones () {
      const vm = this
      const url = `${process.env.VUE_APP_APIPATH}/ringtones`
      vm.$http.get(url).then(response => {
        vm.selectWork = response.data.selectWork
        vm.selectDreak = response.data.selectDreak
      })
    },
    changeRingtones (selectWork, selectDreak) {
      const vm = this
      vm.selectWork = selectWork
      vm.selectDreak = selectDreak
      let data = {
        selectWork,
        selectDreak
      }
      const url = `${process.env.VUE_APP_APIPATH}/ringtones`
      vm.$http.patch(url, data).then(response => {
        console.log(response)
      })
    }
  },
  created () {
    this.getMissioncts()
    this.getRingtones()
    console.log(this.getNotDoneMission)
  },
  computed: {
    getRemainTime () {
      const vm = this
      let minute = parseInt(vm.reciprocal.reciprocalTime / 60)
      let second = (vm.reciprocal.reciprocalTime % 60)
      minute = (minute < 10) ? '0' + minute : minute
      second = (second < 10) ? '0' + second : second
      return `${minute}:${second}`
    },
    getNotDoneMission () {
      const vm = this
      return vm.mission.filter((item) => {
        return !item.done
      })
    },
    getDoneMission () {
      const vm = this
      return vm.mission.filter((item) => {
        return item.done
      })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
