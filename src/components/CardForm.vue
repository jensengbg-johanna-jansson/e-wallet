<template>
    <div class="form--container">
    <form class="form">        
        <label>Card number</label>
        <div class="input input--container">
            <input class="cardnumber-input" ref="cardNumber01" maxlength="4" v-on:input="updateCard" v-model="cardNumber.input01" />
            <input class="cardnumber-input" ref="cardNumber02" maxlength="4" v-on:input="updateCard" v-model="cardNumber.input02" />
            <input class="cardnumber-input" ref="cardNumber03" maxlength="4" v-on:input="updateCard" v-model="cardNumber.input03" />
            <input class="cardnumber-input" ref="cardNumber04" maxlength="4" v-on:input="updateCard" v-model="cardNumber.input04" />
            <ErrorIcon v-if="validCard.initiateValidation && !validCard.cardnumber.isValid" v-bind:errorMsg="validCard.cardnumber.messages" />
        </div>

        <label>Cardholder name</label>
        <div class="input input--container">
            <input type="text" placeholder="Firstname lastname" ref="cardName" v-on:input="updateCard" />
            <ErrorIcon v-if="validCard.initiateValidation && !validCard.cardholder.isValid" v-bind:errorMsg="validCard.cardholder.messages" />
        </div>

        <label class="short-form-item">Valid thru</label>
        <div class="input short-form-item input--container">
            <input type="text" placeholder="MM" class="date-input" ref="dateMonth" v-on:input="updateCard" maxlength="2" v-model="cardMonth" />
            <span>/ </span>
            <input type="text" placeholder="YY" class="date-input" ref="dateYear" v-on:input="updateCard" maxlength="2" />
            <ErrorIcon v-if="validCard.initiateValidation && !validCard.validthru.isValid" v-bind:errorMsg="validCard.validthru.messages" />
        </div>

        <label class="short-form-item">ccv</label>
        <div class="input short-form-item input--container">
            <input type="text" class="ccv-input" ref="ccv" maxlength="3">
            <ErrorIcon v-if="validCard.initiateValidation && !validCard.ccv.isValid" v-bind:errorMsg="validCard.ccv.messages" />
        </div>

        <label class="last-item">Vendor</label>
        <div class="select-box last-item">
            <select ref="vendor" v-on:change="updateCard">
                <option value="0">Select vendor:</option>
                <option value="bitcoin">Bitcoin Inc</option>
                <option value="blockchain">Block Chain Inc</option>
                <option value="evil">Evil Corp</option>
                <option value="ninja">Ninja Bank</option>
            </select>
        </div>
    </form>
    <span v-if="success" class="success-msg">Success! Your card has been added</span>
    <PrimButton v-on:click.native="addCard" v-bind:buttonText="'add card'" v-bind:buttonStyleDark="true" />
    </div>
</template>

<script>
import PrimButton from '../components/PrimaryButton'
import ErrorIcon from '../components/ErrorIcon'
export default {
    name: 'CardForm',
    components: {
        PrimButton,
        ErrorIcon
    },
    props: {
        value: Object,
        resetInput: Boolean,
        success: Boolean
    },
    data: () => {
        return {
            backgroundColor: '',
            cardNumber: {
                input01: '',
                input02: '',
                input03: '',
                input04: ''
            },
            fullCardNumber: '',
            cardMonth: '',
            validCard: {
                initiateValidation: false,
                cardnumber: {
                    isValid: false,
                    checker: {
                        notEmpty: false,
                        isNumber: false,
                        is16length: false
                    },
                    messages: {}
                },
                cardholder: {
                    isValid: false,
                    checker: {
                        notEmpty: false,                    
                        isAlphabetic: false
                    },
                    messages: {}
                },
                validthru: {
                    isValid: false,
                    checker: {
                        notEmpty: false,
                        isNumber: false,
                        is2length: false
                    },
                    messages: {}
                },
                ccv: {
                    isValid: false,
                    checker: {
                        notEmpty: false,
                        isNumber: false,
                        is3length: false
                    },
                    messages: {}
                },
                vendor: {
                    isValid: false,
                    checker: {
                        notEmpty: false
                    },
                    messages: {}
                }
            }
        }
    },
    methods: {
        setCardColor: function() {
            if(this.$refs.vendor.value === 'bitcoin') {
                this.backgroundColor = '#FFAE34'
            } else if(this.$refs.vendor.value === 'blockchain') {
                this.backgroundColor = '#8B58F9'
            } else if(this.$refs.vendor.value === 'ninja') {
                this.backgroundColor = '#222222'
            } else if(this.$refs.vendor.value === 'evil') {
                this.backgroundColor = '#F33355'
            } else {
                this.backgroundColor = '#D0D0D0'
            }
        },
        updateCard: function() {
            this.fullCardNumber = this.$refs.cardNumber01.value + ' ' + this.$refs.cardNumber02.value + ' ' + this.$refs.cardNumber03.value + ' ' + this.$refs.cardNumber04.value;
            
            this.setCardColor();
            this.$emit('input', {
                cardnumber: this.fullCardNumber,
                cardholder: this.$refs.cardName.value,
                month: this.$refs.dateMonth.value,
                year: this.$refs.dateYear.value,
                vendor: this.$refs.vendor.value,
                background: this.backgroundColor
            });           
        },
        deleteMessages: function(field) {
            const keys = Object.keys(this.validCard[field].messages);

            for(let i = 0; i < keys.length; i++) {
                if(Object.prototype.hasOwnProperty.call(this.validCard[field].messages, keys[i])) {
                    this.$delete(this.validCard[field].messages, keys[i]);
                }  
            }
        },
        checkIfValid: function(field) {
            const values = Object.values(this.validCard[field].checker);
            let errorCounter = 0;

            for(let i = 0; i < values.length; i++) {
                if(values[i] == false) {
                    errorCounter++;
                }
            }

            if(errorCounter > 0) {
                this.validCard[field].isValid = false;
            } else {
                this.validCard[field].isValid = true;
            }
        },
        valCardNumber: function() {
            let validator = /^[0-9 0-9]+$/;
            let trimmedCardNumber = this.fullCardNumber.replace(/\s/g,'');

            this.deleteMessages('cardnumber');

            if(this.fullCardNumber != '') {
                this.validCard.cardnumber.checker.notEmpty = true;
            } else {
                this.validCard.cardnumber.checker.notEmpty = false;
                this.$set(this.validCard.cardnumber.messages, 'msgEmpty', 'This field can not be empty');
            }
            
            if(validator.test(this.fullCardNumber)) {
                this.validCard.cardnumber.checker.isNumber = true;
            } else {
                this.validCard.cardnumber.checker.isNumber = false;
                this.$set(this.validCard.cardnumber.messages, 'msgNumber', 'Can only include numbers');
            }

            if(trimmedCardNumber.length === 16) {
                this.validCard.cardnumber.checker.is16length = true;
            } else {
                this.validCard.cardnumber.checker.is16length = false;
                this.$set(this.validCard.cardnumber.messages, 'msgLength', 'Cardnumber should be 16 digits');
            }   

            this.checkIfValid('cardnumber');
        },
        valCardHolder: function() {
            let validator = /^[a-öA-Ö a-öA-Ö]+$/;
            this.deleteMessages('cardholder');

            if(validator.test(this.$refs.cardName.value)) {
                this.validCard.cardholder.checker.isAlphabetic = true;
            } else {
                this.validCard.cardholder.checker.isAlphabetic = false;
                this.$set(this.validCard.cardholder.messages, 'msgAlphabetic', 'Cardholder name should be alphabetic letters');
            }

            if(this.$refs.cardName.value != '') {
                this.validCard.cardholder.checker.notEmpty = true;
            } else {
                this.validCard.cardholder.checker.notEmpty = false;
                this.$set(this.validCard.cardholder.messages, 'msgEmpty', 'This field can not be empty');
            }

            this.checkIfValid('cardholder');
        },
        valDate: function() {
            let validator = /^[0-9]+$/;
            this.deleteMessages('validthru');

            if(
                this.$refs.dateMonth.value != '' ||
                this.$refs.dateYear.value != ''
            ) {
                this.validCard.validthru.checker.notEmpty = true;
            } else {
                this.validCard.validthru.checker.notEmpty = false;
                this.$set(this.validCard.validthru.messages, 'msgEmpty', 'This field can not be empty');
            }

            if(
                validator.test(this.$refs.dateMonth.value) ||
                validator.test(this.$refs.dateYear.value)
            ) {
                this.validCard.validthru.checker.isNumber = true;
            } else {
                this.validCard.validthru.checker.isNumber = false;
                this.$set(this.validCard.validthru.messages, 'msgNumber', 'Can only include numbers');
            }

            if(
                this.$refs.dateMonth.value.length === 2 &&
                this.$refs.dateYear.value.length === 2
            ) {
                this.validCard.validthru.checker.is2length = true;
            } else {
                this.validCard.validthru.checker.is2length = false;
                this.$set(this.validCard.validthru.messages, 'msgLength', 'Cardnumber should be 4 digits');
            }

            this.checkIfValid('validthru');
        },
        valCcv: function() {
            let validator = /^[0-9]+$/;
            this.deleteMessages('ccv');

            if(this.$refs.ccv.value != '') {
                this.validCard.ccv.checker.notEmpty = true;
            } else {
                this.validCard.ccv.checker.notEmpty = false;
                this.$set(this.validCard.ccv.messages, 'msgEmpty', 'This field can not be empty');
            }

            if(validator.test(this.$refs.ccv.value)) {
                this.validCard.ccv.checker.isNumber = true;
            } else {
                this.validCard.ccv.checker.isNumber = false;
                this.$set(this.validCard.ccv.messages, 'msgNumber', 'Can only include numbers');
            }

            if(this.$refs.ccv.value.length === 3) {
                this.validCard.ccv.checker.is3length = true;
            } else {
                this.validCard.ccv.checker.is3length = false;
                this.$set(this.validCard.ccv.messages, 'msgLength', 'Cardnumber should be 3 digits');
            }

            this.checkIfValid('ccv');
        },
        valVendor: function() {
            this.deleteMessages('vendor');

            if(this.$refs.vendor.value != 0) {
                this.validCard.vendor.checker.notEmpty = true;
            } else {
                this.validCard.vendor.checker.notEmpty = false;
                this.$set(this.validCard.vendor.messages, 'msgEmpty', 'Choose a vendor');
            }

            this.checkIfValid('vendor')
        },
        validateInput: function() {                      
            this.valCardNumber();
            this.valCardHolder();
            this.valDate();
            this.valCcv();
            this.valVendor();

            if (
                this.validCard.cardnumber.isValid === true &&
                this.validCard.cardholder.isValid === true &&
                this.validCard.validthru.isValid === true &&
                this.validCard.ccv.isValid === true &&
                this.validCard.vendor.isValid === true
            ) {
                return true;
            } else {
                return false;
            }
        },
        addCard: function() {
            this.validCard.initiateValidation = true;

            if(this.validateInput()) {
                this.$emit('addCard', true);
            } else {
                this.$emit('addCard', false);
            }
        }

    },
    watch: {
        cardNumber: {
            handler() {
                if(this.cardNumber.input01.length === 4) {
                    this.$refs.cardNumber02.focus();
                }
                if(this.cardNumber.input02.length === 4) {
                    this.$refs.cardNumber03.focus();
                }
                if(this.cardNumber.input03.length === 4) {
                    this.$refs.cardNumber04.focus();
                }
            },
            deep: true
        },
        cardMonth: function() {
            if(this.cardMonth.length === 2) {
                this.$refs.dateYear.focus();
            }
        },
        resetInput: function() {
            if(this.resetInput === true) {
                let inputFields = ['cardNumber01', 'cardNumber02', 'cardNumber03', 'cardNumber04', 'cardName', 'dateMonth', 'dateYear', 'ccv', 'vendor'];

                for(let i = 0; i < inputFields.length; i++) {
                    if(inputFields[i] === 'vendor') {
                        this.$refs[inputFields[i]].value = 0;
                    } else if(inputFields[i] === 'dateMonth') {
                        this.$refs[inputFields[i]].value = '';
                        this.cardMonth = '';
                    } else if(inputFields[i] === 'cardNumber01') {
                        this.$refs[inputFields[i]].value = '';
                        this.cardNumber.input01 = '';
                        this.cardNumber.input02 = '';
                        this.cardNumber.input03 = '';
                        this.cardNumber.input04 = '';
                    } else {
                        this.$refs[inputFields[i]].value = '';
                    }
                }

                this.updateCard();
            }
        }
    }
}
</script>

<style scoped lang="scss">
    .form--container {
        width: 100%;
    }
    .form {
        width: 100%;
        display: grid;
        grid-template-columns: repeat(2, calc(50% - 0.5rem));
        grid-column-gap: 1rem;

        input, label, .select-box, .input--container {
            grid-column: span 2;
        }
        label {
            font-size: 0.75rem;
            text-transform: uppercase;
        }
        .input {
            font-size: 1.125rem;
            text-transform: uppercase;
            font-family: 'PT Mono';
            padding: 0.8rem;
            margin-bottom: 1rem;
            border: 1px solid rgba(0, 0, 0, 0.6);
            border-radius: 0.3rem;
        }
        .input--container {
            display: flex;
            
            input {
                font-size: 1.125rem;
                font-family: 'PT Mono';
                text-transform: uppercase;
                border: none;
            }
            .cardnumber-input {
                width: 3.5rem;
            }
            .date-input {
                width: 1.5rem;
            }
            .ccv-input {
                width: 2.5rem;
            }
        }
        
        .select-box {
            position: relative;
            display: block;
            margin-bottom: 1rem;
            border: 1px solid rgba(0, 0, 0, 0.6);
            border-radius: 0.3rem;

            select {
                border: none;
                outline: none;
                background: transparent;
                -webkit-appearance: none;
                -moz-appearance: none;
                appearance: none;
                border-radius: 0;
                margin: 0;
                display: block;
                width: 100%;
                padding: 0.8rem;
                font-size: 1.125rem;
                text-transform: uppercase;
                font-family: 'PT Mono';
            } 
            &:after {
                position: absolute;
                right: 0;
                top: 0;
                width: 50px;
                height: 100%;
                line-height: 38px;
                content: '\2228';
                text-align: center;
                font-size: 24px;
                z-index: -1;
            }
        }

        .short-form-item {
            grid-column: span 1;            
        }
        label.short-form-item {
            order: 2;
        }
        input.short-form-item, div.short-form-item {
            order: 3;
        }

        .last-item {
            order: 4;
        }
    }
    .success-msg {
        display: block;
        width: 100%;
        padding: 0.5rem;
        background: #90ee90;
        border: 1px solid #376e37;
        border-radius: 0.5rem;
        color: #376e37;
        font-family: 'Source Sans Pro';
        font-size: 0.8rem;
        font-weight: bold;
    }
</style>