<template>
  <div class="monster-filters pa-1">

    <v-text-field
      class="mb-1"
      v-model="search"
      label="Filtrer"
      single-line
      hide-details
      color="accent"
      ></v-text-field>

    <v-expansion-panels multiple flat hover v-model="panels">

      <v-expansion-panel>
        <v-expansion-panel-header>Types</v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-btn class="mb-2 mt-2" small color="primary" @click="resetTypes" dark>Réinitialiser</v-btn>
          <v-switch v-for="(type, idx) in types" v-model="type.value" :label="type.label" dense class="ma-0" @change="switchType" color="accent"></v-switch>
        </v-expansion-panel-content>
      </v-expansion-panel>

      <v-expansion-panel>
        <v-expansion-panel-header>Tailles</v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-btn class="mb-2 mt-2" small color="primary" @click="resetSizes" dark>Réinitialiser</v-btn>
          <v-switch v-for="size in sizes" v-model="size.value" :label="size.label" dense class="ma-0" @change="switchSize" color="accent"></v-switch>
        </v-expansion-panel-content>
      </v-expansion-panel>

      <v-expansion-panel>
        <v-expansion-panel-header>Environnements</v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-btn class="mb-2 mt-2" small color="primary" @click="resetEnvironments" dark>Réinitialiser</v-btn>
          <v-switch v-for="(environment, idx) in environments" v-model="environment.value" :label="environment.label" dense class="ma-0" @change="switchEnvironment" color="accent"></v-switch>
        </v-expansion-panel-content>
      </v-expansion-panel>

      <v-expansion-panel>
        <v-expansion-panel-header>Types de donjons</v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-btn class="mb-2 mt-2" small color="primary" @click="resetDungeonTypes" dark>Réinitialiser</v-btn>
          <v-switch v-for="(dungeonType, idx) in dungeonTypes" v-model="dungeonType.value" :label="dungeonType.label" dense class="ma-0" @change="switchDungeonType" color="accent"></v-switch>
        </v-expansion-panel-content>
      </v-expansion-panel>

    </v-expansion-panels>

  </div>
</template>

<script>
// import { mapMutations } from 'vuex'
import { setUrlParams, getUrlParameter, setBooleanMutation, setListMutation } from '@theme/util/filterHelpers'

export default {
  name: 'MonsterFilters',

  data () {
    return {
      panels: [],
    }
  },

  computed: {
    search: {
      get () {
        return this.$store.state.monsterFilters.search
      },
      set (newValue) {
        this.$store.commit('monsterFilters/setSearch', newValue)
      }
    },

    types: {
      get () {
        return this.$store.state.monsterFilters.types
      },
      set (newValue) {
        this.$store.commit('monsterFilters/setTypes', newValue)
      }
    },

    sizes: {
      get () {
        return this.$store.state.monsterFilters.sizes
      },
      set (newValue) {
        this.$store.commit('monsterFilters/setSizes', newValue)
      }
    },

    environments: {
      get () {
        return this.$store.state.monsterFilters.environments
      },
      set (newValue) {
        this.$store.commit('monsterFilters/setEnvironments', newValue)
      }
    },

    dungeonTypes: {
      get () {
        return this.$store.state.monsterFilters.dungeonTypes
      },
      set (newValue) {
        this.$store.commit('monsterFilters/setDungeonTypes', newValue)
      }
    },
  },

  methods: {

    switchType () {
      let list = []
      for (var i = 0; i < this.types.length; i++) {
        if (this.types[i].value) {
          list.push(this.types[i].label)
        }
      }
      setUrlParams('types', list)
    },

    switchSize () {
      let list = []
      for (var i = 0; i < this.sizes.length; i++) {
        if (this.sizes[i].value) {
          list.push(this.sizes[i].label)
        }
      }
      setUrlParams('tailles', list)
    },

    switchEnvironment () {
      let list = []
      for (var i = 0; i < this.environments.length; i++) {
        if (this.environments[i].value) {
          list.push(this.environments[i].label)
        }
      }
      setUrlParams('environnements', list)
    },

    switchDungeonType () {
      let list = []
      for (var i = 0; i < this.dungeonTypes.length; i++) {
        if (this.dungeonTypes[i].value) {
          list.push(this.dungeonTypes[i].label)
        }
      }
      setUrlParams('donjons', list)
    },

    setColor (value, compare, color) {
      if (value === compare) {
        return color
      }
      return this.$site.themeConfig.primaryColor
    },

    resetTypes () {
      this.$store.commit('monsterFilters/resetTypes')
      this.switchType()
    },

    resetSizes () {
      this.$store.commit('monsterFilters/resetSizes')
      this.switchSize()
    },

    resetEnvironments () {
      this.$store.commit('monsterFilters/resetEnvironments')
      this.switchEnvironment()
    },

    resetDungeonTypes () {
      this.$store.commit('monsterFilters/resetDungeonTypes')
      this.switchDungeonType()
    },
  },

  mounted () {
    this.$store.dispatch('monsterFilters/reset')

    let selectedSizes = getUrlParameter(window.location.href, "tailles").split(",")
    let selectedTypes = getUrlParameter(window.location.href, "types").split(",")
    let selectedEnvironments = getUrlParameter(window.location.href, "environnements").split(",")
    let selectedDungeonTypes = getUrlParameter(window.location.href, "donjons").split(",")

    setListMutation(selectedTypes, this.$store, 'monsterFilters/setTypesFromList')
    setListMutation(selectedSizes, this.$store, 'monsterFilters/setSizesFromList')
    setListMutation(selectedEnvironments, this.$store, 'monsterFilters/setEnvironmentsFromList')
    setListMutation(selectedDungeonTypes, this.$store, 'monsterFilters/setDungeonTypesFromList')
  }
}
</script>

<style lang="scss">
</style>
