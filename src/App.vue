<template>
    <div id="app">
        <h1>Let's add fractions</h1>
        <hr>
        <div class="form-sum-fractions">
            <Fraction
                    v-for="fraction in fractions"
                    :id="fraction.id"
                    :numerator="fraction.numerator"
                    :denominator="fraction.denominator"
                    :operator="fraction.operator"
                    :key="fraction.id"
                    @removeFraction="removeFraction"
                    @updateFraction="updateFraction"
            />
            <div class="result">
                <div>
                    =
                </div>
                <div>
                    {{fractionsResult}}
                </div>
            </div>
        </div>
        <button v-on:click="addFraction">
            add new element
        </button>
    </div>
</template>

<script>
    import Fraction from "./components/Fraction";

    export default {
        name: 'App',
        components: {
            Fraction,
        },
        data() {
            return {
                fractions: [
                    {id: 1, numerator: null, denominator: null, operator: null},
                    {id: 2, numerator: null, denominator: null, operator: '+'},
                ],
                config: {
                    maxCountFractions: 5,
                    minCountFractions: 2
                }
            }
        },
        methods: {
            addFraction() {
                if (this.fractions.length < this.config.maxCountFractions) {
                    this.fractions.push({
                        id: Date.now(),
                        numerator: null,
                        denominator: null,
                        operator: '+'
                    })
                } else {
                    alert(`Максимальное количество дробей ограничено (${this.config.maxCountFractions})`)
                }
            },
            updateFraction(id, field, value) {
                let isDone = false
                const newFractions = this.fractions.map(fraction => {
                    if (fraction.id === id) {
                        isDone = true
                        return {
                            ...fraction,
                            [field]: value
                        }
                    } else {
                        return fraction
                    }
                })
                if (isDone) {
                    this.fractions = newFractions
                } else {
                    console.error(`updateFraction(${id}) fail: Элемент с данным ID не найден`)
                }
            },
            removeFraction(id) {
                const newFractions = this.fractions.filter(fraction => fraction.id !== id)
                if (newFractions.length !== this.fractions.length) {
                    if (newFractions.length < this.config.minCountFractions) {
                        alert(`Минимальное количество дробей ограничено (${this.config.minCountFractions})`)
                    } else {
                        this.fractions = newFractions
                    }
                } else {
                    console.error(`removeFraction(${id}) fail: Элемент с данным ID не найден`)
                }
            }
        },
        computed: {
            fractionsResult: function () {
                if (this.fractions) {
                    const validFractions = this.fractions.filter(({ numerator, denominator }) => numerator !== null && denominator !== null)
                    if (validFractions.length === this.fractions.length) {
                        let result = 0
                        validFractions.forEach(({ numerator, denominator }) => {
                            result+= Number(numerator) / Number(denominator)
                        })
                        return result.toFixed(2)
                    } else {
                        return 'заполните'
                    }
                }
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .form-sum-fractions {
        display: flex;
        flex-direction: row;
        justify-content: center;
        margin-left: calc(50% - 188px);
        max-width: 375px;
        margin-bottom: 2rem;
    }

    .result {
        display: flex;
    }

    .result div {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        font-size: 2rem;
        padding: .2rem;
    }
</style>
