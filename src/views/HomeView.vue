<template>
  <Timer />
</template>

<script>
import Timer from "@/components/Timer.vue"

export default {
  components: { Timer},

  setup() {

  },

  emits: ["toggleMode"]

}
</script>

<style>

</style>