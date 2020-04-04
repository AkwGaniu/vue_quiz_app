<template>
  <div>
    <b-nav>
      <b-nav-item disabled>Fancy Quiz</b-nav-item>
      <b-nav-item disabled>Question(s) answered: {{ totalAnswered }} / {{ totalCorrect}} </b-nav-item>
    </b-nav>    
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: ['totalAnswered', 'totalCorrect']
}
</script>