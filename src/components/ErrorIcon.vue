<template>
    <div class="error--container">
        <span class="error-icon" v-on:mouseover="hover = true" v-on:mouseleave="hover = false">
            !
        </span>
        <div class="error-box" v-if="hover">
            <ul>
                <li v-for="(msg, index) in errorMsg" :key="index">{{ msg }}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ErrorIcon',
    props: {
        errorMsg: Object
    },
    data () {
        return {
            hover: false
        }
    }
}
</script>

<style scoped lang="scss">
    .error--container {
        position: relative;
        margin-left: auto;
    }
    .error-icon {
        background: red;
        color: #ffffff;
        font-weight: bolder;
        font-size: 1.125rem;
        border-radius: 50%;
        padding: 0.2rem 0.5rem;
        cursor: default;
    }
    .error-box {
        position: absolute;
        top: -0.9rem;
        left: 2.8rem;
        z-index: 1;
        width: 20vw;
        color: #ffffff;
        background: red;
        border: 1px solid red;
        padding: 0.7rem;
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
        border-radius: 0.3rem;
        
        ul {
            list-style-position: inside;
        }
        li {
            font-size: 0.75rem;
            font-family: 'Source Sans Pro';
            text-transform: initial;
        }
    }
    .error-box:after {
        content: '';
        position: absolute;
        left: 0;
        top: 1rem;
        width: 0;
        height: 0;
        border: 19px solid transparent;
        border-right-color: red;
        border-left: 0;
        margin-top: -10px;
        margin-left: -10px;
    }
</style>