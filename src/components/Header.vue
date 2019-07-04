<template>
  <v-toolbar>
    <v-toolbar-title class="headline text-uppercase">
      <span>Quiz</span>
      <span class="font-weight-light">Vue test</span>
    </v-toolbar-title>
    <v-spacer></v-spacer>
      <span class="mr-2">Counter: {{ numCorrect }}/{{ numTotal }}</span>
  </v-toolbar>
</template>
<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>
