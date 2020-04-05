<template>
    <div class="question-box-container">
        <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
            <template v-slot:lead>
               {{ currentQuestion.question }}
            </template>

            <hr class="my-4">


            <b-list-group v-for="(answer,index) in answers" :key="index">
              <b-list-group-item @click="selectAnswer(index)"
                                 :class="[selectedIndex===index ? 'selected' :'']">{{ answer }}</b-list-group-item>
            </b-list-group>


            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex===null">Submit</b-button>
            <b-button @click="next" variant="success">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'
    export default {
        props: {
            currentQuestion: {
                incorrect_answers: [],
                correct_answer: Object
            },
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                isSubmitted: false,
                shuffleAnswers: []
            }
        },
        computed: {
            answers(){
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer);
                return answers;
            }
        },
        watch: {
            /*
              when currentQuestion is change will execute this method
             */
            currentQuestion :{
                immediate: true,
                handler(){
                    this.selectedIndex = null;
                    this.shuffleAnswer();
                    this.isSubmitted = false;
                }
            }
        },
        methods:{
            selectAnswer(index){
                this.selectedIndex = index;
            },
            shuffleAnswer(){
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
                this.shuffleAnswers = _.shuffle(answers);
                this.correctIndex = this.shuffleAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer(){
                this.increment(this.selectedIndex === this.correctIndex);
                this.next(this.isSubmitted);
                this.isSubmitted = true;
            }
        }
    }
</script>

<style scoped>
    .list-group{
        margin-bottom: 10px;
    }
    .list-group-item:hover{
        background: #EEEEEE;
        cursor: pointer;
    }
    .btn{
        margin: 0 5px;
    }
    .selected {
        background: lightblue;
    }
    .correct{
        background: lightgreen;
    }
    .incorrect{
        background: lightcoral;
    }
</style>