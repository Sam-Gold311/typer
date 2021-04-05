<template>
  <div class='key' :class='this.somekey.space ? spaceClass : ""'>
    {{ this.keyText }}
  </div>
</template>


<script>

  export default {
    name: 'Key',
    props: {
      somekey: Object,
      uppercase: Boolean
    },
    data() {
      return {
        spaceClass: 'key_space'
      }
    },
    computed: {
      keyText() {
        if(this.uppercase) {
          return this.somekey.upText;
        } else {
          return this.somekey.lowText;
        }
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
    box-sizing: border-box;
    border: 2px solid black;
    border-radius: 5px;
    transition: all .1s ease-in;
    &_space {
      margin-top: 5px;
      width: 200px;
    }
    &_correct {
      background: #69F0AE;
      transition: all .1s ease-in;
    }
    &_wrong {
      background: #EF5350;
      transition: all .1s ease-in;
    }
  }
</style>
