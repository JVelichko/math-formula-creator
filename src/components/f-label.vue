<template>
  <div class="label">
    <template v-if="type === 'number'">
      <input type="number" v-model="val" @input="setNumber">
    </template>
    <template v-else>
      {{content}}
    </template>
    <span class="label__remove" @click="remove">
    </span>
  </div>
</template>

<script>
  export default {
    name: "field-label",
    props: {
      content: String,
      id: String,
      type: String,
    },
    data() {
      return {
        val: null,
      }
    },
    methods: {
      setNumber() {
        this.$emit('on-input', this)
      },
      remove() {
        this.$emit('remove', this.id )
      }

    }
  }
</script>


<style scoped lang="sass">
  $color-blue: #306BFF

  .label
    background-color: #ffffff
    padding: 0 12px
    border: 1px solid $color-blue
    color: $color-blue
    height: 32px
    line-height: 30px
    border-radius: 4px
    display: flex
    align-items: center
    justify-content: center
    position: relative

    input
      width: 64px
      border-radius: 2px
      border: 1px solid #424242

    &__remove
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' width='16px' height='16px' viewBox='0 0 48 48' fill='%23ff0000'%3e%3cpath d='M38 12.83L35.17 10 24 21.17 12.83 10 10 12.83 21.17 24 10 35.17 12.83 38 24 26.83 35.17 38 38 35.17 26.83 24z' stroke='%23ff0000' stroke-width='4px'/%3e%3cpath d='M0 0h48v48H0z' fill='none'/%3e%3c/svg%3e ")
      background-position: center
      background-repeat: no-repeat
      height: 16px
      padding: 4px
      width: 16px
      position: absolute
      opacity: 0.8
      right: -6px
      top: -6px


</style>