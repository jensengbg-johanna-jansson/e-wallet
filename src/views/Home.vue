<template>
  <section class="home main--container">
    <h1 class="main__heading">E-wallet</h1>
    <div class="active-card">
      <h2 class="heading">Active card</h2>
      <transition name="fade">
        <CardActionMenu v-if="hover" v-on:click.native="deleteCard()" v-on:mouseover.native="menuHover = true" v-on:mouseleave.native="menuHover = false" />
      </transition>
      <Card v-if="cardList.length != 0" v-bind:cardInfo="activeCard" v-on:mouseover.native="cardHover = true" v-on:mouseleave.native="mouseOutDelay" class="layer" />
      <NoCards v-else />
    </div>
    <CardStack v-bind:cardList="cardList" v-on:changeActiveCard="changeActiveCard" />
    <PrimButton v-on:click.native="goToAdd" v-bind:buttonText="'Add a new card'" />
  </section>
</template>

<script>
// @ is an alias to /src
import Card from '../components/Card'
import CardStack from '../components/CardStack.vue'
import PrimButton from '../components/PrimaryButton'
import CardActionMenu from '../components/CardActionMenu'
import NoCards from '../components/NoCards'

export default {
  name: 'Home',
  components: {
    Card,
    CardStack,
    PrimButton,
    CardActionMenu,
    NoCards
  },
  props: {
    cardList: Array
  },
  data: () => {
    return {
      activeCard: {},
      hover: false,
      cardHover: false,
      menuHover: false,
      updateCardList: false
    }
  },
  methods: {
    changeActiveCard: function(value) {
      this.activeCard = value;
    },
    goToAdd: function() {
      this.$router.push('add');
    },
    mouseOutDelay: function() {
        setTimeout(() => { this.cardHover = false }, 300);
    },
    setHover: function() {
      if(this.cardHover === false && this.menuHover === false) {
        this.hover = false;
      } else {
        this.hover = true;
      }
    },
    deleteCard: function() {
      let currentCardId = this.activeCard.id;
      
      let newCardList = this.cardList.filter(function(card) {
        return card.id !== currentCardId;
      });
      localStorage.setItem('cards', JSON.stringify(newCardList));
      this.updateCardList = true;
      this.$emit('updateCardList', this.updateCardList);
      
      this.hover = false;
    }
  },
  mounted() {
      this.activeCard = this.cardList[0];
  },
  computed: {
    currentCard: function() {
      return this.cardList[0];
    }
  },
  watch: {
    menuHover: function() {
      this.setHover();
    },
    cardHover: function() {
      this.setHover();
    },
    cardList: function() {
      this.activeCard = this.cardList[0];
    }
  }
}
</script>

<style scoped lang="scss">
  .main--container {
    width: 380px;
  }
  .active-card {
      margin-bottom: 1.5rem;
      position: relative;
      width: 100%;

      .layer {
        position: relative;
        z-index: 1;
      }

      .heading {
          font-family: 'Source Sans Pro';
          font-size: 0.75rem;
          font-weight: 300;
          text-transform: uppercase;
          text-align: center;
          margin-bottom: 0.5rem;
      }
  }

  .fade-enter-active {
    transition: all .3s ease;
  }
  .fade-leave-active {
    transition: all .8s ease;
  }
  .fade-enter, .fade-leave-to {
    transform: translateX(-2rem);
    opacity: 0;
  }
</style>