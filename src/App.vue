<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <TextInput 
      label="First Name" 
      error="Only characterse are allowed for a first name"
      required pattern="\w+" 
      ref="firstname" />
    <TextInput 
      label="Email" 
      error="You must supply valid email address!"
      required type="email" 
      ref="lastname" />
    <button v-on:click="validate">Validate</button>
  </div>
</template>

<script>
import TextInput from './components/TextInput.vue'

export default {
  name: 'App',
  components: {
    TextInput
  },
  methods: {
    validate: function () {
      let promises = [];
      Object.keys(this.$refs).forEach(ref => {
        promises.push(this.$refs[ref].validate());
      });
      Promise.all(promises).then((valid) => {
        if (valid.every((v) => v.isValid)) {
          console.log('valid!');
        }
        else {
          console.log('invalid');
          valid.filter((v) => !v.isValid).forEach((v) => console.log(v.error));
        }
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
