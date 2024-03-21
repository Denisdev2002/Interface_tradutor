<template>
  <div>
    <p>{{ translatedText }}</p>
  </div>
</template>

<script>
export default {
  props: {
    translatedText: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
</style>
