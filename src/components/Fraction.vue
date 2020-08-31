<template>
    <div class="item">
        <div class="operator">
            {{operator}}
        </div>
        <div class="fraction">
            <div><input type="text" v-mask="numMask" v-on:input="onInputNumerator" v-model="numeratorValue"></div>
            <div><input type="text" v-mask="numMask" v-on:input="onInputDenominator" v-model="denominatorValue"></div>
            <button v-on:click="$emit('removeFraction', id)">&times;</button>
        </div>
    </div>
</template>

<script lang="js">
    import Vue from 'vue'
    import { VueMaskDirective } from 'v-mask'
    import _ from 'lodash'

    Vue.directive('mask', VueMaskDirective)

    export default {
        name: "Fraction",
        props: ['id', 'numerator', 'denominator', 'operator'],
        data() {
            return {
                numMask: [/[1-9]/, /[0-9]/],
                numeratorValue: '',
                denominatorValue: ''
            }
        },
        methods: {
            onInputNumerator: _.debounce(function () {
                this.$emit('updateFraction', this.id, 'numerator', this.numeratorValue === '' ? null : Number(this.numeratorValue))
            }, 500),
            onInputDenominator: _.debounce(function () {
                this.$emit('updateFraction', this.id, 'denominator', this.denominatorValue === '' ? null : Number(this.denominatorValue))
            }, 500)
        }
    }
</script>

<style scoped>
    .item {
        display: flex;
    }

    .item:first-child .operator {
        display: none;
        width: 0;
        height: 0;
        margin: 0;
        padding: 0;
    }

    .fraction, .operator {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .operator {
        font-size: 2rem;
        padding: .2rem;
    }

    .fraction div {
        padding: .2rem;
    }

    .fraction div:first-child {
        border-bottom: 2px solid #2c3e50;
    }

    .fraction input {
        display: block;
        width: 1.5rem;
        height: 1.5rem;
        text-align: center;
    }
</style>