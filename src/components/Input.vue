<template>
  <div class='todo__textbox'>
    <input
      @change="this.$emit('inputValue', value)"
      placeholder="Enter the task"
      max="100"
      class='todo__inputbox'
      v-model='value'
      :class="{ 'font-dark' : darkMode, 'font-light' : !darkMode }"
    />
  </div>
</template>
  
<style>
  .todo__inputbox {
    border: 0;
    padding: 10px 14px;
    border-radius: 6px;
    width: 100%;
  }

  .todo__inputbox::placehoder{
    color: #B4B4B4;
  }
</style>
  
<script>
export default {
  props: {
    darkMode: Boolean
  },
  data() {
    return {
      value: ''
    }
  },
  methods: {
    clearInputValue(){
      this.value = ''
    }
  }
}
</script>
