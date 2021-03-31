<template>
  <div class='key' v-ripple="{ class: 'success--text' }" >
    {{ somekey.keyText }}
  </div>
</template>


<script>

  export default {
    name: 'Key',
    props: {
      somekey: Object
    },
    data() {
      return {

      }
    },
    methods: {
      keyPressed() {
        this.$el.mousedown();
      }
    }
  }
</script>





<style media="screen" lang='scss'>
  .key {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    border: 1px solid black;
    border-radius: 5px;
  }
</style>
