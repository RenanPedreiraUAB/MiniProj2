<template>
  <section class="page-section">
    <b-container fluid>
      <HeaderPage title="Gestão do Animalec"/>

      <b-row>
        <b-col cols="3"></b-col>
        <b-col cols="6">
          <b-card-group deck>
            <AdminOptionBox routeName="listUsers" iconName="users" title="Utilizadores" />
            <AdminOptionBox routeName="listAnimals" iconName="dog" title="Animais" />
            <AdminOptionBox routeName="listQuizzes" iconName="file-alt" title="Quizzes" />
          </b-card-group>
          <b-card-group deck class="mt-3">
            <AdminOptionBox routeName="listQuestions" iconName="question-circle" title="Questões" />
            <AdminOptionBox routeName="listSponsors" iconName="handshake" title="Patrocinadores" />
            <AdminOptionBox routeName="listExperts" iconName="user-md" title="Especialistas" />
          </b-card-group>
        </b-col>
        <b-col cols="3"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import HeaderPage from "@/components/HeaderPage.vue";
import AdminOptionBox from "@/components/AdminOptionBox.vue";

export default {
  components: {
    HeaderPage,
    AdminOptionBox,
  },
};
</script>
