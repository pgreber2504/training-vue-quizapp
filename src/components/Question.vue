<template>
    <div>
        <b-jumbotron>
            <template v-slot:lead>
            {{question.question}}
            </template>

            <hr class="my-4">

            <p>
                <b-list-group>
                    <b-list-group-item  :class="answerClass(index)" @click.prevent="select(index)" button v-for="(answer, index) in shuffledAnswers" :key="index">{{answer}}</b-list-group-item>
                </b-list-group>
            </p>

            <b-button :disabled="selectedIndex === null || answered" variant="primary" @click="submitAnswer" href="#">Check</b-button>
            <b-button :disabled="number === 10" variant="success" @click="next" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>

import lodash from 'lodash'

export default {
    props:{
        question: Object,
        next: Function,
        number: Number,
        increment: Function
    },
    data(){
        return{
            index: true,
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
        }
    },
    methods:{
        select(id){
            this.selectedIndex = id;
        },

        submitAnswer(){
            let isCorrect = false;

             if(this.shuffledAnswers[this.selectedIndex] === this.question.correct_answer){
                isCorrect = true;
            }

            this.answered = true;

            this.increment(isCorrect);

        },

        shuffle(){
            let arr = [...this.question.incorrect_answers, this.question.correct_answer];
            this.shuffledAnswers = lodash.shuffle(arr);
            this.correctIndex = this.shuffledAnswers.indexOf(this.question.correct_answer)
        },

        answerClass(index){
            let answeredClass = null;

            if(!this.answered && this.selectedIndex === index){
                answeredClass = "selected";
            }else if (this.answered && this.correctIndex === index ){
                answeredClass = 'success'
            }else if (this.answered && this.correctIndex ==! index && this.selectedIndex === index){
                answeredClass = 'error'
            }

            return answeredClass
        }
    },
    computed:{
        answers(){
            let answers = [...this.question.incorrect_answers];
            answers.push(this.question.correct_answer);
            return answers
        }

    },
    watch:{
        question:{
            immediate: true,
            handler(){
                this.selectedIndex = null,
                this.answered = false,
                this.shuffle()
            }
        }
        
    }
}
</script>

<style scoped>
   div{
        text-align: center;
    }

    .btn{
        margin: 0 5px;
    }
    .list-group-item:hover{
        background-color: #eee;
        cursor: pointer;
    }

    .selected{
        background-color: lightblue;
    }

    .success{
        background-color: lightgreen;
    }

    .error{
        background-color: salmon;
    }
</style>