<template>
  <v-row class="fill-height" justify="center" align="center">
    <v-col align="center">
      <v-row align="center" justify="center" class="mb-3">
        <v-avatar size="150">
          <v-img :src="require('assets/profile.jpg')" />
        </v-avatar>
      </v-row>
      <v-row align="center" justify="center">
        <h1>Bastián Matamala</h1>
      </v-row>
      <v-row align="center" justify="center" class="mt-12" no-gutters>
        <v-card max-width="700" flat outlined>
          <v-card-text>
            I am currently a Master's student in Computer Science, and my thesis is related to the use of an NLP approach
            to analyze the correlation between different domains of headline misinformation (clickbait, incongruity, fake news).
            My current professional interests are web development and the applications of deep learning,
            with additional points if both worlds can meet.
          </v-card-text>
        </v-card>
      </v-row>
      <v-row align="center" justify="center" class="mt-12">
        <v-btn x-large icon href="https://github.com/bma98" class="mr-6">
          <v-icon x-large>
            mdi-github
          </v-icon>
        </v-btn>
        <v-btn x-large icon href="mailto:bastian.matamala@ug.uchile.cl">
          <v-icon x-large>
            mdi-email
          </v-icon>
        </v-btn>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'WelcomeSheet'
}
</script>

<style scoped>

</style>
