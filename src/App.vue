<template>
  <ScoreBoardComposition :user-count="userCount " :computer-count="computerCount"/>
  <QuestionsBoardComposition @updateScores="handleScoresUpdate" />
</template>

<script>
//import ScoreBoard from '@/components/ScoreBoard.vue';
//import QuestionsBoard from '@/components/QuestionsBoard.vue';
import ScoreBoardComposition from '@/components/ScoreBoardComposition.vue';
import QuestionsBoardComposition from '@/components/QuestionsBoardComposition.vue';

export default {
  name: 'App',
  components:{
    //ScoreBoard,
    //QuestionsBoardComposition
    ScoreBoardComposition,
    QuestionsBoardComposition
  },
  data() {
    return {
      userCount: 0,
      computerCount: 0
    };
  },
  methods: {
    handleScoresUpdate(newScores) {
      this.userCount = newScores.userCount;
      this.computerCount = newScores.computerCount;
    }
  }
}
</script>