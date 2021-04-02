<template>
  <div class='key'>
    {{ somekey.keyText }}
  </div>
</template>


<script>

  export default {
    name: 'Key',
    props: {
      somekey: Object,
      nextLetter: String
    },
    data() {
      return {

      }
    },
    methods: {
      keyPressed(e) {
        console.log(e);
        if (e.key === this.nextLetter) {
          this.$el.classList.add('key_correct');
          setTimeout(() => this.$el.classList.remove('key_correct'), 100);
          this.$emit('change-text');
        } else {
          this.$el.classList.add('key_wrong');
          setTimeout(() => this.$el.classList.remove('key_wrong'), 100);
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
    border: 2px solid black;
    border-radius: 5px;
    transition: all .1s ease-in;
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
