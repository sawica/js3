<template>
  <div id="edit">
    <label> {{ dog.name }} </label>
    <input
      type="text"
      v-model="dogName"
      aria-label=""
      placeholder="imię">
    <label> {{ dog.birthdate }} </label>
    <input
      type="date"
      v-model="dogBirthdate"
      aria-label=""
      placeholder="data urodzenia">
  </div>
</template>

<script>
export default {
  name: 'DogEdit',
  data () {
    return {
      dogName: '',
      dogId: '',
      dogBirthdate: ''
    }
  },
  async created () {
    try {
      // eslint-disable-next-line no-undef
      import(edit)
    } catch (e) {
      console.error(e)
    }
  },
  methods: {
  }
}
</script>

<style scoped>

</style>
