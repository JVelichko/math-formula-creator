<template>
  <button :value="value" class="btn" @click="addLabel">{{type === 'number' ? 'Число' : value}}</button>
</template>

<script>
  export default {
    name: "f-button",
    props: {
      type: String,
      value: String,
    },

    computed: {

    },
    methods: {
      addLabel: function () {
        this.$emit('add-label', this.type, this.value);
      }

    }
  }
</script>

<style scoped lang="sass">
  $color-blue: #306BFF
  $color-gray: #c0c0c0
  $color-white: #ffffff

  .btn
    background-color: $color-white
    border: 1px solid $color-blue
    color: $color-blue
    height: 32px
    border-radius: 4px


    &:not(:last-child)
      margin-bottom: 16px
</style>