<template>
    <section class="add-card main--container">
        <h1 class="main__heading">Add a new bank Card</h1>
        <div class="card-wrapper">
            <h2 class="heading">New card</h2>
            <Card v-bind:cardInfo="form" />
        </div>
        <CardForm v-model="form" v-on:addCard="updateAddCardValue" v-bind:resetInput="resetInput" v-bind:success="success" />
        <PrimButton v-on:click.native="goToAdd" v-bind:buttonText="'back'" />
    </section>
</template>

<script>
import Card from '../components/Card'
import CardForm from '../components/CardForm'
import PrimButton from '../components/PrimaryButton'
export default {
    name: 'AddCard',
    components: {
        Card,
        CardForm,
        PrimButton
    },
    props: {
        cardList: Array
    },
    data: () => {
        return {
            form: {
                id: '',
                cardnumber: '0000 0000 0000 0000',
                cardholder: 'firstname lastname',
                month: 'mm',
                year: 'yy',
                background: '#D0D0D0'
            },
            addCard: false,
            resetInput: false,
            success: false
        }
    },
    methods: {
        goToAdd: function() {
            this.$router.push('/');
        },
        updateAddCardValue (value) {
            this.addCard = value;
        },
        cardID: function() {
            if(this.cardList.length === 0 ) {
                return 1;
            } else {
                return parseInt(this.cardList[this.cardList.length-1].id) + 1;
            }
        }
    },
    watch: {
        form: function() {            
            if(this.form.cardnumber === '   ') {
                this.form.cardnumber = '0000 0000 0000 0000'
            }
            if(this.form.cardholder === '') {
                this.form.cardholder = 'firstname lastname'
            }
            if(this.form.month === '') {
                this.form.month = 'mm'
            }
            if(this.form.year === '') {
                this.form.year = 'yy'
            }
        },
        addCard: function() {
            if(this.addCard === true) {
                this.form.id = this.cardID();
                this.cardList.push(this.form);
                localStorage.setItem('cards', JSON.stringify(this.cardList));
                this.resetInput = true;
                this.success = true;
            } else {
                console.log('Something went wrong. Could not add card');
                this.success = false;
            }
        }
    }
}
</script>

<style scoped lang="scss">
    .add-card {
        width: 380px;

        .card-wrapper {
            margin-bottom: 1.5rem;
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
</style>