<template>
    <div>
        <b-jumbotron>

            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                v-for="(ans, index) in shuffledAnswer" 
                :key="index"
                @click="selectAnswer(index)"
                :class="selectClass(index)"
                >
                {{ ans }}
                </b-list-group-item>
            </b-list-group>


            <b-button variant="primary"
                @click="submitAnswer"
                :disabled="selected ===  null || answered"
                >Submit</b-button>
            <b-button variant="success" @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    name: 'questionBox',
    props:{
        currentQuestion: Object,
        next: Function,
        increaseCount: Function
    },

    data() {
        return {
            selected: null,
            correctIndex: null,
            shuffledAnswer: [],
            answered: false,
            isCorrect: false,
        }
    },

    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selected = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },

    methods: {
        selectAnswer(index) {
            this.selected = index
        },
        
        shuffleAnswers() {
            this.shuffledAnswer = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswer = _.shuffle(this.shuffledAnswer)
        },

        submitAnswer() {
            this.correctIndex = this.shuffledAnswer.indexOf(this.currentQuestion.correct_answer)
            this.answered = true
            this.correctIndex === this.selected ? this.isCorrect = true : this.isCorrect = false
        
            this.increaseCount(this.isCorrect)

        },

        selectClass(index) {
            let selectedClass = null

            if (this.answered && index === this.correctIndex) {
                selectedClass = 'correct'
            }
            else if (this.answered && index === this.selected &&  index !== this.correctIndex) {
                selectedClass = 'wrong'

            } 
            else if(index === this.selected) {
                selectedClass = 'selected'

            }
            return selectedClass
        }
    },

    computed: {
        answers() {
            let ans = [...this.currentQuestion.incorrect_answers]
            ans.push(this.currentQuestion.correct_answer)
            return ans
        }
    }
}
</script>

<style scoped>
    .list-group-item {
        margin-bottom: 10px;
        cursor: pointer
    }
    

    .btn {
        margin: 0 10px;
    }
    .selected {
        background: lightblue
    }

    .correct {
        background: lightgreen
    }

    .wrong {
        background: lightcoral;
    }
</style>