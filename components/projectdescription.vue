<template>
  <v-row justify="center" align="center">
    <v-col>
      <v-row class="mt-12 mb-12">
        <v-col align="center">
          <h1>Projects</h1>
        </v-col>
      </v-row>
      <v-row align="center" justify="center">
        <v-col cols="10">
          <v-expansion-panels>
            <v-expansion-panel>
              <v-expansion-panel-header class="font-weight-bold">
                Movie History
              </v-expansion-panel-header>
              <v-expansion-panel-content>
                <v-card flat>
                  <v-list align="left">
                    <v-card-text>
                      <v-list-item>
                        Webapp designed to keep a historical record of movies watched.
                      </v-list-item>
                      <v-list-item>
                        A backend service developed with Django REST Framework acts as a PostgreSQL
                        database driver to maintain the movie entities, the people working on them, the users and the
                        history itself. It exposes several endpoints in order to provide functionality for listing,
                        getting details and creating objects.
                      </v-list-item>
                      <v-list-item>
                        A frontend service developed with VueJS and Vuetify that consumes the backend service, allowing
                        the user to see their history record, the details of the movies watched, the details of the people
                        involved and basic interaction such as ranking movies and adding new entries to the history.
                      </v-list-item>
                      <v-list-item>
                        The data that feeds the database is obtained from The Movie Database.
                      </v-list-item>
                    </v-card-text>
                  </v-list>
                  <v-card-actions>
                    <v-btn text href="https://github.com/BMA98/bmovies-backend">
                      <v-icon size="30">
                        mdi-github
                      </v-icon>
                      BACK
                    </v-btn>
                    <v-btn text href="https://github.com/BMA98/bmovie-new">
                      <v-icon size="30">
                        mdi-github
                      </v-icon>
                      FRONT
                    </v-btn>
                    <v-spacer/>
                    <v-btn text href="https://movies.bastian.cl">
                      DEMO
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'ProjectDescription'
}
</script>

<style scoped>

</style>
