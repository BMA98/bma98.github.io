<template>
  <div>
    <v-sheet :height="h">
      <welcome/>
    </v-sheet>
    <v-sheet :height="h">
      <projectdescription/>
    </v-sheet>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => ({
    h: window.innerHeight
  })
}
</script>
