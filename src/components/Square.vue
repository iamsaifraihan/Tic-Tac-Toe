<template>
  <button class="square" :class="[ value, { 'winner': winner } ]"
    :disabled="disabled" @click="click">{{value}}</button>
</template>

<script>
export default {
  name: 'Square',
  props: {
    value: String,
    winner: Boolean,
    disabled: Boolean
  },
  methods: {
    click () {
      this.$emit('click')
    }
  }
}
</script>

<style scoped>
.square {
  background: transparent;
  border: 1px solid #fffc;
  font-size: 15vmin;
  font-weight: bold;
  line-height: 34px;
  padding: 0;
  text-align: center;
  transition: all .25s ease;
  outline: none;
}

.square.winner {
  background: #349c858a;;
}

.square:not([disabled]):empty:hover,
.square:not([disabled]):empty:focus {
  box-shadow: inset 0 0px 14px #140a2785;
  cursor: pointer;
}

.square:not([disabled]):empty:active {
  box-shadow: inset 0 2px 50px #0008;
}

.square.X {
  color: #f34444;
}

.square.O {
  color: #ff9627;
}
</style>
