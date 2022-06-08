<template>
  <button
      @click="$emit('onClick')"
      class="TheButton"
    >
      {{ text }}
    </button>
</template>

<script>
export default({
  props: {
    text: { 
      type: String,
      default: "ボタン",
      required: false 
    }
  },
  setup(context) {
    const execEmit = () => {
      context.emit('onClick');
    }
    return {
      execEmit
    }
  }
})
</script>