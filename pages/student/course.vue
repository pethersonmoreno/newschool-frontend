<template>
  <div v-if="loading" class="spiner-container">
    <v-progress-circular
      :size="70"
      :width="5"
      indeterminate
    ></v-progress-circular>
    <client-only>
      <navigation-bar />
    </client-only>
  </div>
  <not-found v-else-if="notFound" />
  <v-layout v-else-if="!notFound" justify-center>
    <v-flex ref="flex" class="course-page" xs10 sm8 md6>
      <v-container class="course-content">
        <v-row class="page-top-bar">
          <v-col cols="12" class="relative-col">
            <v-btn class="btn-back" text icon @click="gotoIndex">
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
            <h2 class="page-title">CURSO</h2>
          </v-col>
        </v-row>
        <v-row>
          <h3 class="course-title">{{ course.title }}</h3>
        </v-row>
        <v-row>
          <p class="course-description-title">Descrição do curso</p>
        </v-row>
        <v-row>
          <v-img :src="course.thumbUrl" />
        </v-row>
        <v-row>
          <span class="teacher-title">Professor(a)</span>
          <span class="teacher">{{
            course.authorId || 'Marcela Tavares'
          }}</span>
        </v-row>
        <v-row>
          <p class="course-description">{{ course.description }}</p>
        </v-row>
      </v-container>
      <v-container class="bottom-button-container">
        <v-row>
          <v-btn
            class="btn-block btn-submit btn-primary"
            depressed
            large
            @click="start"
            >Iniciar</v-btn
          >
        </v-row>
      </v-container>
    </v-flex>
    <client-only>
      <navigation-bar />
    </client-only>
  </v-layout>
</template>

<script>
import NotFound from '../public/404.vue'
import NavigationBar from '~/components/NavigationBar.vue'
import courses from '~/services/http/courses'

export default {
  components: {
    NavigationBar,
    NotFound,
  },
  data() {
    return {
      loading: true,
      notFound: false,
      course: null,
    }
  },
  mounted() {
    const { slug } = this.$route.params
    courses
      .getBySlug(slug)
      .then(({ data }) => {
        console.log('data: ', data)
        const { id, authorId, description, slug, thumbUrl, title } = data
        this.course = { id, authorId, description, slug, thumbUrl, title }
        this.loading = false
      })
      .catch(error => {
        if (error.response && error.response.status === 404) {
          this.notFound = true
          this.loading = false
          return
        }
        // eslint-disable-next-line no-console
        console.error(error)
      })
  },
  methods: {
    start() {
      const { slug } = this.$route.params
      $nuxt._router.push(`/aluno/curso/${slug}/start`)
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Montserrat:400,500,900&display=swap');
@import url('https://fonts.googleapis.com/css?family=Lato&display=swap');

/* Global */
* {
  font-family: 'Montserrat', Helvetica, Arial, sans-serif !important;
}

.flex {
  animation: intro 300ms backwards;
  animation-delay: 350ms;
}

.layout {
  background: #fff !important;
}

/* Spinner */
.spiner-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}

/* Placeholder */
::v-deep ::placeholder {
  color: #6600cc !important;
}
.course-page {
  display: flex;
  flex-direction: column;
}
.course-content {
  flex: 1;
}

.bottom-button-container {
  flex: 0 1 160px;
}

/* Page */
.page-top-bar {
  margin-bottom: 32px;
}
.page-title {
  font-family: 'Lato', sans-serif;
  font-style: normal;
  font-weight: 900;
  font-size: 16px;
  text-transform: uppercase;
  line-height: 19px;
  text-align: center;
  color: #6600cc;
}
.course-title {
  font-style: normal;
  font-weight: 900;
  font-size: 14px;
  line-height: 17px;
  display: flex;
  align-items: center;
  color: #6600cc;
  margin-bottom: 4px;
}
.course-description-title {
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
  /* identical to box height */
  display: flex;
  align-items: flex-end;
  color: #656565;
  margin-bottom: 8px !important;
}
.teacher-title {
  font-style: normal;
  font-weight: bold;
  font-size: 14px;
  line-height: 17px;
  display: flex;
  align-items: center;
  color: #6600cc;
}
.teacher {
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
  display: flex;
  align-items: flex-end;
  color: #1a1a1a;
  margin-left: 5px;
}
.course-description {
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 15px;
  display: flex;
  align-items: flex-end;
  color: #656565;
  margin-top: 8px;
}

.relative-col {
  position: relative;
}

/* Botão */
::v-deep .btn-back {
  position: absolute;
  left: 0;
  top: 0;
  margin-top: 3px;
  font-size: 12px;
}
@media (max-width: 350px) {
  ::v-deep .btn-back {
    position: absolute;
    left: -25px;
    top: 0;
    margin-top: 3px;
    font-size: 12px;
  }
}

::v-deep .v-btn.v-size--large {
  font-size: 12px;
  line-height: 15px;
}

@media (max-width: 375px) {
  ::v-deep .v-btn {
    font-size: inherit;
  }
}

/* Form */
.v-form {
  width: 100%;
}

::v-deep .theme--light.v-label {
  font-weight: 600;
  color: #6600cc !important;
}

::v-deep .btn-back .theme--light.v-icon {
  color: #60c;
  font-size: 25px;
}

::v-deep .v-icon.v-icon.v-icon--link {
  color: #aa56ff;
  padding-right: 5px;
}

::v-deep
  .btn-primary.theme--light.v-btn:not(.v-btn--flat):not(.v-btn--text):not(.v-btn--outlined) {
  background: #6600cc !important;
  border-radius: 0 !important;
  box-shadow: 0 4px 5px gray !important;
  color: #fff !important;
  font-weight: bold !important;
}
</style>
