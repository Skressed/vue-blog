<template>
  <div id="app">
    <pgheader></pgheader>
    <modals v-bind:posts="cards"></modals>
    <cards v-bind:posts="cards"></cards>
  </div>
</template>

<script>
import Pgheader from './components/Pgheader.vue'
import Cards from './components/Cards.vue'
import Modals from './components/Modals.vue'
import mockedData from './fixtures/mockData.json'
import { bus } from './main'

export default {
  name: 'App',
  components: {
    Pgheader,
    Cards,
    Modals
  },
  data () {
    return {
      defaultCards: mockedData,
      cards: [],
    }
  },
  created () {
    bus.$on('loadData', () => {
      this.cards = this.pickLocalStorageData();
    });
    this.cards = this.pickLocalStorageData();

  },
  methods: {
    pickLocalStorageData: function() {
      const lsCards = window.localStorage.getItem('cards');
      if(!lsCards){
        window.localStorage.setItem('cards', JSON.stringify(this.defaultCards));
        return this.defaultCards;
      }
      return JSON.parse(lsCards);
    }
  }
}
</script>

<style>

</style>
