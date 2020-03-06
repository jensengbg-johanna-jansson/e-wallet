<template>
  <div id="app">
    <router-view v-bind:cardList="cardList" v-on:updateCardList="updateCardList" />
  </div>
</template>

<script>
  import defaultcards from './assets/default-cards.json'
  export default {
    data() {
      return {
        cardList: '',
        createNewCardList: false
      }
    },
    methods: {
      updateCardList: function(value) {
        this.createNewCardList = value;
      },
      createCardList: function() {
        if(localStorage.getItem('cards')) {
          this.cardList = JSON.parse(localStorage.getItem('cards'));
        } else {
          localStorage.setItem('cards', JSON.stringify(this.defaultcards));
          this.cardList = JSON.parse(localStorage.getItem('cards'));
        }
      }
    },
    created() {
      this.createCardList();
    },
    computed: {
        defaultcards() {
            return defaultcards.defaultcards;
        }
    },
    watch: {
      createNewCardList: function() {
        if(this.createNewCardList === true) {
          this.createCardList();
          
          this.createNewCardList = false;
        }
      }
    }
  }
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

p {
  font-size: 1.125rem;
}

#app {
  font-family: 'PT Mono';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem 0;
}

.main__heading {
  font-family: Source Sans Pro;
  text-transform: uppercase;
  font-size: 2.175rem;
  margin-bottom: 1rem;
}

.main--container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

</style>
