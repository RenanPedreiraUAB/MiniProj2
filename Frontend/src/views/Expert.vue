<template>
    <section class="page-section">
      <b-container>
        <HeaderPage :title="expert.name" />
        <b-row>
          <b-col>
            <b-card class="overflow-hidden" style="max-width: 1540px;">
              <b-row>
                <b-col md="6" align="center">
                  <b-card-img :src="expert.photoUrl" class="rounded-0 mb-2"></b-card-img>
                </b-col>
                <b-col md="6">
                  <b-card-body :title="expert.name + ' (' + expert.animaliaFamily + ')'">
                    <b-card-text align="left">
                      <b>Descrição:</b>
                      {{ expert.description }}
                    </b-card-text>
                    <b-card-text>
                      <b>Email de Contato:</b>
                      <a :href="'mailto:' + expert.contactInfo">{{ expert.contactInfo }}</a>
                    </b-card-text>
                    <b-card-text>
                      <b>Link:</b>
                      <a :href="expert.link" target="_blank">{{ expert.link }}</a>
                    </b-card-text>
                    <router-link
                      :to="{ name: 'experts' }"
                      tag="b-button"
                      variant="outline-danger"
                      align="center"
                      class="mr-2"
                    >
                      <i class="fas fa-arrow-alt-circle-left"></i> VOLTAR
                    </router-link>
                  </b-card-body>
                </b-col>
              </b-row>
            </b-card>
          </b-col>
        </b-row>
      </b-container>
    </section>
  </template>
  
  <script>
  import HeaderPage from "@/components/HeaderPage.vue";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Expert",
    components: {
      HeaderPage,
    },
    data() {
      return {
        expert: "",
      };
    },
    computed: {
      ...mapGetters("expert", ["getExpertsById"]),
    },
    created() {
      this.expert = this.getExpertsById(this.$route.params.expertId);
    },
  };
  </script>
  