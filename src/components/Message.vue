<template>
  <div>
    {{ from }}: {{ text }} <sub>{{ createdAt }}</sub>
  </div>
</template>
<script>
export default {
  props: {
    from: String,
    text: String,
    createdAt: Number
  }
}
</script>
