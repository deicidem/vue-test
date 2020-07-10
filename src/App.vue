<template>
<div class="container">
  <div class="progress">
    <div class="progress-bar" role="progressbar" :style="width"></div>
  </div>
  <hr>
  <transition name="fade" mode="out-in" key="quiz">
  <div v-if="progress != quests.length">
  <transition-group  name="fade" mode="out-in">
    <component :is="item.type" class="question" v-show="index == progress"  v-for="(item, index) in quests" :title="item.title" :answers="item.answers" :key="item.title" @submit="onSubmit(index, $event)"></component>
  </transition-group>
  </div>
  <div class="result" key="result" v-else>
    <h2>Итого</h2>
    <hr>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Вопрос</th>
          <th>Ваш ответ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in result" :key="index">
          <td>{{item.title}}</td>
          <td>
            Правильно: {{item.correct}} из {{item.need}}
            <ul class="list-group list-group-flush">
              <li class="list-group-item" :class="elem.correct ? 'list-group-item-success' : 'list-group-item-danger'" v-for="(elem, index) in item.answer" :key="index">
                {{elem.name}}
              </li>
            </ul>
          </td>
        </tr>
      </tbody>

    </table>
  </div>
  </transition>
</div>
</template>

<style scoped>
.container {
  padding: 15px;
}
.question {
  position: absolute;
}
.fade-enter {
  opacity: 0;
}
.fade-enter-active{
  transition: opacity 0.5s;
}
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-leave-to {
  opacity: 0;
}
.question.fade-enter-active {
  transition-delay: .5s;
}
/* .fade-leave-active{
  transition: opacity 1s;
} */
</style>

<script>
import Checkbox from "./components/Checkbox";
import Radio from "./components/Radio";

function getQuests() {
  return [{
      type: 'radio',
      title: 'Какой тег задаёт ссылку?',
      answers: [
        'a',
        'div',
        'span',
        'img'
      ],
      correct: [
        'a'
      ]
    },
    {
      type: 'checkbox',
      title: 'Какие из  этих тегов строчные?',
      answers: [
        'a',
        'div',
        'span',
        'img'
      ],
      correct: [
        'a',
        'span'
      ]
    }
  ];
}
export default {
  data() {
    return {
      quests: getQuests(),
      progress: 0,
      result: []
    }
  },
  created() {
    this.quests.forEach(item => {
      this.result.push({
        title: item.title,
        answer: [],
        need: item.correct.length,
        correct: 0
      });
    })
  },
  components: {
    Radio,
    Checkbox
  },
  methods: {
    onSubmit(index, ans) {
      let correct = this.quests[index].correct;
      let result = [];
      ans.forEach(item => {
        if (item.value) {
          let isCorrect = false;
          let count = 0;
          if (correct.indexOf(item.name) != -1) {
            isCorrect = true;
            this.result[index].correct++;
          }
          result.push({
            name: item.name,
            correct: isCorrect,
          });
        }
      })

      this.result[index].answer = result;
      this.progress++;
    }
  },
  computed: {
    width() {
      return {
        width: this.progress / this.quests.length * 100 + '%'
      }
    }
  }
}
</script>
