<template>
    <section class="card--container" v-bind:class="textClass" v-bind:style="{ backgroundColor: cardInfo.background }">
        <div class="card-icon--container">
            <img src="../assets/chip-light.svg" alt="" class="card-chip__icon">
            <img :src="vendor" alt="" class="card-vendor__icon">
        </div>
        <p class="card-number">{{ cardInfo.cardnumber }}</p>
        <div class="card-info--container">
            <h2 class="card-info__heading">Cardholder Name</h2>
            <p>{{ cardInfo.cardholder }}</p>
            <h2 class="card-info__heading">Valid Thru</h2>
            <p>{{ cardInfo.month }}/{{ cardInfo.year }}</p>
        </div>
    </section>
</template>

<script>
export default {
    name: 'Card',
    props: {
        cardInfo: Object
    },
    data: () => {
        return {
            vendor: '',
            textClass: ''
        }
    },
    methods: {
        setTextClass: function() {
            if(this.cardInfo.vendor === 'bitcoin' ||
                this.cardInfo.vendor == null ||
                this.cardInfo.vendor == 0
            ) {
                this.textClass = 'dark__p';
            } else {
                this.textClass = 'light__p';
            }
        },
        emptyCardInfo: function() {

        }
    },
    mounted() {
        this.setTextClass();

        if(this.cardInfo.vendor == 0 || this.cardInfo.vendor == null) {
            this.vendor = ''; 
        } else {
            this.vendor = require('../assets/vendor-' + this.cardInfo.vendor + '.svg'); 
        }
    },
    watch: {
        cardInfo: function() {
            if(this.cardInfo.vendor == 0) {
                this.vendor = ''; 
            } else {
                this.vendor = require('../assets/vendor-' + this.cardInfo.vendor + '.svg'); 
            }

            this.setTextClass();
        }
    }
}
</script>

<style scoped lang="scss">
    .card--container {
        width: 380px;
        height: 240px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        border-radius: 0.5rem;
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.4);
        padding: 1.5rem 1rem;        
    }
        .dark__p p {
            color: #3B3B3B;
            text-shadow: -1px -1px #FFFFFF, 0.3px 0.3px #000000;
        }
        .light__p p {
            color: #ffffff;
            text-shadow: 0.3px 0.3px #3B3B3B; 
        }
        .dark__p h2 {
            color: #000000;
        }
        .light__p h2 {
            color: #FFFFFF;
        }

    .card-icon--container {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
    }
    .card-number {
        font-size: 1.8125rem;
        margin-bottom: 1rem;
    }
    .card-info--container {
        display: grid;
        grid-template-columns: 1fr 90px;
        grid-template-rows: repeat(2, 1fr);
        grid-auto-flow: column; 
    }
    .card-info__heading {
        font-size: 0.75rem;
        text-transform: uppercase;
        font-weight: 300;
        color: #000000;
    }
</style>