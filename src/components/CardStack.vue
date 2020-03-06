<template>        
    <div class="card-stack" :style="height">
        <h2 v-if="emptyStack">You have no more cards</h2>
        <Card v-for="(card, index) in stackList" :key="index"
            v-bind:cardInfo="card"
            v-on:click.native="openCard(card.id)"
            :ref="'card'"
            :style="{ transform: 'translate(0, -' + stackPosition[index] + ')' }"
        />
    </div>
</template>

<script>
import Card from '../components/Card'
export default {
    name: 'CardStack',
    components: {
        Card
    },
    props: {
        cardList: Array
    },
    data: () => {
        return {
            stackPosition: [],
            changeActiveCard: '',
            activeCardId: '',
            emptyStack: false
        }
    },
    methods: {
        openCard: function(cardId) {
            let activeCard = this.cardList.find(function(card) {
                                return card.id == cardId;
                            });
            this.changeActiveCard = activeCard;
            this.activeCardId = this.changeActiveCard.id;
            this.$emit('changeActiveCard', this.changeActiveCard);
        }
    },
    mounted() {
        if(this.cardList.length === 0) {
            this.emptyStack = true;
        } else if(this.cardList.length === 1) {
            this.activeCardId = this.cardList[0].id;

            for(let i = 0; i < this.stackList.length; i++) {
                this.stackPosition.push(i*12 + 'rem');
            }
            this.emptyStack = true;
        } else {
            this.activeCardId = this.cardList[0].id;

            for(let i = 0; i < this.stackList.length; i++) {
                this.stackPosition.push(i*12 + 'rem');
            }

            this.emptyStack = false;
        }
    },
    computed: {
        height() {
            let stackLength = this.stackList.length;
            return { 
                'height': 15 + (stackLength - 1) * 3 + 'rem'
            };
        },
        stackList: function() {
            let thecard = this.activeCardId;
            return this.cardList.filter(function(u) {
                return u.id != thecard;
            })
        }
    },
    watch: {
        changeActiveCard: function() {
        },
        cardList: function() {
            if(this.cardList.length === 0) {
                this.emptyStack = true;
            } else if(this.cardList.length ===1) {
                this.emptyStack = true
                this.activeCardId = this.cardList[0].id;
            } else {
                this.activeCardId = this.cardList[0].id;
                this.emptyStack = false;
            }
        }
    }
}
</script>

<style scoped lang="scss">
    .card-stack {
        height: var(--height);

        h2 {
            text-align: center;
            font-family: 'Source Sans Pro';
            color: gray;
        }
    }
</style>