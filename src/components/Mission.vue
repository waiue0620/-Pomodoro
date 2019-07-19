<template>
  <div>
    <section>
      <div class="pb-5">
        <input class="input text-primary border-0 font-italic pl-3" type="text" placeholder="ADD A NEW MISSION…" v-model.trim="newMission" @keydown.enter="addNewMission">
        <i class="material-icons inputIcons bg-white text-primary cursor-pointer" @click="addNewMission">add</i>
      </div>
      <div class="d-flex cursor-pointer h4 px-3 py-2 whiteOpacity20 mb-3" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
        to-do
        <i class="material-icons align-self-center ml-auto transition rotate180">arrow_drop_down</i>
      </div>
      <draggable id="collapseOne" class="p-0 collapse show overflow-auto" v-model="tempMission" @start="todoListStart" @end="sendToParentEnd" :move='todoListCheckMove' group='notDoneMission' style="max-height:238px;">
        <div class="list" v-for="(item, index) in tempMission" :key="index" :id="item.id">
          <div class="d-flex missionList pb-2 mb-2 cursor-pointer" v-if="!item.done">
            <div class="done done-md done-white mr-1" @click="doneMission(item)"></div>
            <p class="m-0 mr-auto cursor-move">{{ item.title}}</p>
            <i class="material-icons cursor-pointer"  @click="changMission(item)">play_circle_outline</i>
          </div>
        </div>
        <!-- <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">the second thing to do today</p>
          <i class="material-icons">play_circle_outline</i>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">the third thing to do today</p>
          <i class="material-icons">play_circle_outline</i>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">the forth thing to do today</p>
          <i class="material-icons">play_circle_outline</i>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">the Forth thing to do today</p>
          <i class="material-icons">play_circle_outline</i>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">complete the keynote</p>
          <i class="material-icons">play_circle_outline</i>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"></div>
          <p class="m-0 mr-auto">prepare presentationy</p>
          <i class="material-icons">play_circle_outline</i>
        </li> -->
      </draggable>
      <draggable class="removeBox text-warning text-center border border-warning py-2 mb-3 d-none" :class="{'text-danger border-danger' : moveRemove, 'd-block' : move} "
      v-model="removeList" group='notDoneMission' ref="todoListRemoveBox">
        <div>DELETE</div>
      </draggable>
      <div class="collapsed d-flex cursor-pointer h4 px-3 py-2 whiteOpacity20 mb-3" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
        done
        <i class="material-icons align-self-center ml-auto transition rotate180">arrow_drop_down</i>
      </div>
      <draggable id="collapseTwo" class="p-0 collapse overflow-auto" v-model="tempMission" @start="todoListStart" @end="sendToParentEnd" :move='todoListCheckMove' group='notDoneMission' style="max-height:238px;">
        <div class="list" v-for="(item, index) in tempMission" :key="index"  :id="item.id" :done='item.done'>
          <div class="d-flex missionList pb-2 mb-2" v-if="item.done">
            <div class="done done-md done-white mr-1 cursor-pointer" @click="doneMission(item)"><i class="material-icons doneIcons">done</i></div>
            <p class="m-0 mr-auto font-italic cursor-move"><s>{{ item.title }}</s></p>
            <div class="d-flex">
              <div class="done done-sm bg-white align-self-center mr-2" v-for="(item, index) in item.tomato" :key="index"></div>
            </div>
          </div>
        </div>
        <!-- <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"><i class="material-icons doneIcons">done</i></div>
          <p class="m-0 mr-auto font-italic"><s>mockup of the new case</s></p>
          <div class="d-flex">
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
          </div>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"><i class="material-icons doneIcons">done</i></div>
          <p class="m-0 mr-auto font-italic"><s>product prototype</s></p>
          <div class="d-flex">
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
          </div>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"><i class="material-icons doneIcons">done</i></div>
          <p class="m-0 mr-auto font-italic"><s>draw a wireframe</s></p>
          <div class="d-flex">
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
          </div>
        </li>
        <li class="d-flex missionList pb-2 mb-2">
          <div class="done done-md done-white mr-1"><i class="material-icons doneIcons">done</i></div>
          <p class="m-0 mr-auto font-italic"><s>website detail refine</s></p>
          <div class="d-flex">
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
            <div class="done done-sm bg-white align-self-center mr-2"></div>
          </div>
        </li> -->
      </draggable>
      <draggable class="removeBox text-warning text-center border border-warning py-2 mb-3 d-none" :class="{'text-danger border-danger' : moveRemove, 'd-block' : doneListmove} "
      v-model="removeList" group='notDoneMission' ref="doneTodoListRemoveBox">
        <div>DELETE</div>
      </draggable>
    </section>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  components: {
    draggable
  },
  props: {
    mission: {
      type: Array,
      default: function () {
        return {}
      }
    }
  },
  data () {
    return {
      newMission: '',
      tempMission: this.mission,
      move: false,
      doneListmove: false,
      removeList: [],
      moveRemove: false
    }
  },
  methods: {
    addNewMission () {
      const vm = this
      console.log(vm.newMission)
      if (vm.newMission === '') { return }
      vm.$emit('addNewMission', vm.newMission)
      vm.newMission = ''
    },
    sendToParentEnd (evt) {
      const vm = this
      vm.$emit('sendToParentMission', vm.tempMission)
      vm.move = false
      vm.doneListmove = false
      if (vm.moveRemove) {
        console.log(evt.clone.id)
        vm.deleteMission(evt.clone.id)
        vm.moveRemove = false
      }
    },
    todoListCheckMove (evt, originalEvent) {
      const vm = this
      // console.log(evt.to == vm.$refs.todoListRemoveBox.$el)
      // console.log(evt.to == vm.$refs.todoListRemoveBox.$el)
      // console.log(evt.to == vm.$refs.doneTodoListRemoveBox.$el)
      // console.log(evt)
      evt.to === vm.$refs.todoListRemoveBox.$el || evt.to === vm.$refs.doneTodoListRemoveBox.$el ? vm.moveRemove = true : vm.moveRemove = false
      // if (evt.to == this.$refs.todoListRemoveBox.$el) { vm.moveRemove = true }
    },
    check () {
      console.log(this.move)
    },
    todoListStart (e) {
      const vm = this
      console.log(e.from.id)
      e.from.id === 'collapseOne' ? vm.move = true : vm.doneListmove = true
      // console.log(e.clone.id)
    },
    deleteMission (id) {
      const vm = this
      const url = `${process.env.VUE_APP_APIPATH}/mission/${id}`
      vm.$http.delete(url).then(response => {
        console.log(response)
      })
    },
    doneMission (item) {
      const vm = this
      vm.$emit('doneMission', item)
    },
    changMission (item) {
      const vm = this
      vm.$emit('changMission', item)
    }
  },
  created () {
  }
}
</script>
