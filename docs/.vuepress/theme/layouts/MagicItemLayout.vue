<template>
  <div class="magic-item">
    <div class="d-flex flex-wrap align-center d-print-none">
      <Breadcrumb class="mr-auto mb-4" />
      <div class="d-flex flex-wrap align-center">
        <v-btn color="primary" class="mr-4 mb-4" depressed link to="/creation-d-objet-magique/"><v-icon left>mdi-plus</v-icon> Créer un objet magique</v-btn>
        <v-btn :outlined="!isMagicItemInTreasureChest" color="accent" class="mr-4 mb-4" depressed @click="toggleMagicItemInTreasureChest"><v-icon>mdi-book</v-icon> {{ displayToggleMagicItemButton }}</v-btn>
        <v-btn color="primary" class="mb-4" depressed link to="/mon-grimoire/">Mes objets magiques</v-btn>
      </div>
    </div>
    <MagicItem :magicItem="$page" />
    <Edit />
  </div>
</template>

<script>
import Breadcrumb from '@theme/components/Breadcrumb'
import MagicItem from '@theme/components/MagicItem'
import Edit from '@theme/components/Edit'

export default {
  name: 'MagicItemLayout',

  components: {
    Breadcrumb,
    MagicItem,
    Edit
  },

  computed: {
    isMagicItemInTreasureChest () {
      let isInTreasureChest = false
      for (let s of this.$store.state.myMagicItems.magicItems) {
        if (s.key == this.$page.key) {
          isInTreasureChest = true
        }
      }
      return isInTreasureChest
    },

    displayToggleMagicItemButton () {
      if (this.isMagicItemInTreasureChest) {
        return 'Supprimer de mes objets magiques'
      }
      return 'Ajouter à mes objets magiques'
    }
  },

  methods: {
    toggleMagicItemInTreasureChest () {
      if (this.isMagicItemInTreasureChest) {
        this.$store.commit('myMagicItems/removeMagicItem', this.$page)
      } else {
        this.$store.commit('myMagicItems/addMagicItem', this.$page)
      }
    }
  },

  mounted () {
    this.$store.commit('setHasRightDrawer', false)
    this.$store.commit('setRightDrawer', false)
    this.$store.commit('setInRightDrawer', null)
  }
}
</script>

<style>
</style>
