<template>
  <div class="input-group input-group-sm input-wrapper">
    <div class="input-group-prepend">
      <button
        class="btn btn-outline-secondary control-button"
        type="button"
        @click="
          inputValue = inputValue > 0 ? Math.round((inputValue - step) * 10) / 10 : 0;
          $emit('input', inputValue);
        "
        :disabled="disable"
      >↓</button>
    </div>
    <input
      type="text"
      class="form-control text-center field_required"
      disabled
      aria-describedby="basic-addon1"
      :value="inputValue + ' mm'"
      :style="{opacity: disable ? .3 : 1}"
    >
    <div class="input-group-append">
      <button
        class="btn btn-outline-secondary control-button"
        type="button"
        @click="
          inputValue = Math.round((inputValue + step) * 10) / 10;
          $emit('input', inputValue);
        "
        :disabled="disable"
      >↑</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'NumberInput',

    props: {
      value: {
        type: Number,
        default: 0
      },
      step: {
        type: Number,
        default: 0.5
      },
      disable: {
        type: Boolean,
        default: false
      },
    },

    data() {
      return {
        inputValue: this.value
      }
    },

    watch: {
      value(newValue) {
        this.inputValue = newValue
      }
    },
  }
</script>

<style lang="sass" scoped>
  @use "~main/theme"

  .input-group.input-wrapper
    .control-button
      border-width: 1px

    .btn-outline-secondary
      border-color: rgb(var(--color-divider))
      background-color: rgb(var(--color-input-background))

      &:hover, &:focus
        color: rgb(var(--color-text-primary))
        background-color: rgb(var(--color-input-background))
        opacity: .8
        outline: none
        box-shadow: none
    

    input
      background-color: rgb(var(--color-input-background))
      border-top-color: rgb(var(--color-divider))
      border-bottom-color: rgb(var(--color-divider))
      color: rgb(var(--color-text-primary))
      width: 5rem
</style>