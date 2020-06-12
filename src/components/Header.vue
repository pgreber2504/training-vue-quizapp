<template>
    <div>
        <b-nav>
            <b-nav-item disabled> <b>QUIZ APP</b> </b-nav-item>
            <b-nav-item disabled>Question: {{number}}/10</b-nav-item>
            <b-nav-item  id="correct" disabled>Correct: {{correctAnswers}}/10</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props:{
        number: Number,
        correctAnswers: Number,
    }
}
</script>

<style scoped>
    #correct a{
        color: green!important;
    }

    
</style>