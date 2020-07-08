<template>
<form @submit.prevent>
  <h2>{{title}}</h2>
  <hr>
  <div class="form-check" v-for="(item, index) in answers" :key="index">
    <label class="form-check-label" >
      <input class="form-check-input" :value="item" type="radio" v-model="answer" >
      {{item}}
    </label>
  </div>
  <hr>
  <button class="btn btn-success"  @click="submit" :disabled="!answer">Далее</button>

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
      answer: false
    }
  },
  methods: {
    submit() {
      let result = [{
        name: this.answer,
        value: true
      }]
      this.$emit('submit', result);
    }
  }
}
</script>
