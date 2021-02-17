<template>
  <v-container fluid>
    <v-navigation-drawer
      permanent
      expand-on-hover
      absolute
      right
    >
      <v-list>
        <v-list-item class="px-2">
          <v-list-item-avatar>
            <v-img src="https://randomuser.me/api/portraits/women/85.jpg"></v-img>
          </v-list-item-avatar>
        </v-list-item>

        <v-list-item link>
          <v-list-item-content>
            <v-list-item-title class="title">
              Sandra Adams
            </v-list-item-title>
            <v-list-item-subtitle>sandra_a88@gmail.com</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-list
        nav
        dense
      >
        <v-list-item link to="/plan" id="nav-element">
          <v-list-item-icon>
            <v-icon>mdi-folder</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Character</v-list-item-title>
        </v-list-item>
        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-account-multiple</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Shared with me</v-list-item-title>
        </v-list-item>
        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-star</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Starred</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    
      <v-row>
        <ul style="list-style: none; display: inline-flex; float: left; margin: 5px 7px;">
          <SelectPlan v-on:selectedPlan="loadPlan" />
          <UploadPlan v-bind:plannedModules="plannedModules" :key="UploadPlanKey"/>
          <v-btn class="mx-1" color="grey lighten-1" x-large rounded v-on:click="removeAll">
            <v-icon large class="mr-2">mdi-delete-sweep</v-icon>
            Clear All
          </v-btn>
        </ul>
      </v-row>
      <v-row>
        <v-col v-for="year in numYears" :key="year" justify="center" class="px-0 mx-0">
          <Year :year="year" :numSem="numSem[year]" :yearModules="plannedModules[year]" v-on:changeModuleList="changeModuleList" :key="componentKey"/>
        </v-col>
      </v-row>
  </v-container>
</template>

<script>
import Year from '../components/Year.vue'
import SelectPlan from '../components/SelectPlan.vue'
import UploadPlan from '../components/UploadPlan.vue'

export default {
  name: 'Plan',
  components: {
    Year,
    SelectPlan,
    UploadPlan
  },
  data () {
    return {
      exportedModules: [],
      numYears: [0, 1, 2, 3],
      numSem: [2, 2, 2, 2],
      plannedModules: [[[],[]],[[],[]],[[],[]],[[],[]]],
      componentKey: 0,
      UploadPlanKey: 0
    }
  },
  mounted () {
    if (localStorage.getItem('plannedModules')) {
      // console.log('EXISTS')
      // console.log(localStorage.getItem('plannedModules'))
      try {
        this.plannedModules = JSON.parse(localStorage.getItem('plannedModules'))
        console.log(this.plannedModules)
      } catch (e) {
        localStorage.removeItem('plannedModules')
      }
    } 
  },
  methods: {

    loadPlan (emitData) {
      this.plannedModules = emitData
      this.saveModuleList ()
      this.componentKey++
      // console.log('loadplan placeholder')
    },

    removeAll () {
      this.plannedModules = [[[],[]],[[],[]],[[],[]],[[],[]]]
      this.saveModuleList ()
      this.componentKey++
      // console.log('removeAll placeholder')
    },

    changeModuleList (eventData) {
      // console.log('MODULELISTCHANGED')
      const yEdited = eventData[0][0] - 1
      const sEdited = eventData[0][1] - 1
      const newModuleCodesList = eventData[1]
      // console.log(newModuleCodesList)
      this.plannedModules[yEdited][sEdited] = newModuleCodesList
      this.saveModuleList()
      this.UploadPlanKey++
    },

    saveModuleList () {
      // console.log('SAVED')
      const parsed = JSON.stringify(this.plannedModules);
      localStorage.setItem('plannedModules', parsed);
      // console.log(localStorage.getItem('plannedModules'))
    }
  }
}
</script>
<style scoped>
</style>
