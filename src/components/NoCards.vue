<template>
    <section class="no-card">
        <article class="message">
            <h2>Hey there!</h2>
            <p>Looks like you don't have any cards.</p>
            <p>Why not add one?</p>
        </article>
        <PrimButton v-on:click.native="addCard" v-bind:buttonText="'add a card'" v-bind:buttonStyleDark="true" />
    </section>
</template>

<script>
import PrimButton from '../components/PrimaryButton'
export default {
    name: 'NoCards',
    components: {
        PrimButton
    },
    methods: {
        addCard: function() {
            this.$router.push('add');
        }
    }
}
</script>

<style lang="scss" scoped>
    .no-card {
        width: 100%;

        h2 {
            text-transform: uppercase;
        }

        p {
            font-family: 'Source Sans Pro';
            margin-bottom: 0.5rem;
        }
    }
</style>>