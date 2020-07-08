<template>
<form @submit.prevent>
  <h2>{{title}}</h2>
  <hr>
  <div class="form-check" v-for="(item, index) in answers" :key="index">
    <label class="form-check-label" >
      {{answer[index][item]}}
      <input class="form-check-input"  type="checkbox" v-model="answer[index].value" >
      {{item}}
    </label>
  </div>
  <hr>
  <button class="btn btn-success"  @click="submit" :disabled="disable">Далее</button>
</form>
</template>

<script>
export default {
  props: {
    title: String,
    answers: Array,
  },
  data() {
    return{
      answer: [],
    }
  },
  computed: {
    disable() {
      let result = true;
      for (let i = 0; i < this.answer.length; i++) {
        if (this.answer[i].value) {
          result = false
        } 
      }
      return result;
    }
  },
  created() {
    for(let i = 0; i < this.answers.length; i++) {

      this.answer.push({
        name: this.answers[i],
        value: false
      });
      
    }
  },
  methods: {
    submit() {
      this.$emit('submit', this.answer);
    }
  }
}
</script>
