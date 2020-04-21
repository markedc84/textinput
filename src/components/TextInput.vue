<template>
  <div>
    <label ref="label">
      {{label}}
      <input 
        v-bind:pattern="dirty ? pattern : null" 
        v-bind:required="dirty ? required : null" 
        v-bind:type="type"
        v-on:blur="onBlur"
        ref="input" />
        <span>
          {{error}}
        </span>
    </label> 
  </div>
</template>

<script>
export default {
  name: 'TextInput',
  props: {
    label: String,
    error: String,
    type: String,
    required: Boolean,
    pattern: String,
  },
  data: function () {
    return {
      dirty: false
    }
  },
  methods: {
    onBlur: function () {
      this.dirty = true;
    },
    validate: function () {
      var that = this;
      return new Promise(function(resolve) { 
          that.dirty = true;
          that.$nextTick(function () {
            var valid = that.$refs["input"].checkValidity();
            resolve({
              isValid: valid,
              error: valid ? null : that.error
            });
          });
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  border: 2px solid black;
  padding: 5px;
}
input:invalid {
  border: 2px solid red;
}
input + span {
  color: red;
}
input:valid + span {
  display: none;
}
label {
  font-weight: bold;
}
</style>
