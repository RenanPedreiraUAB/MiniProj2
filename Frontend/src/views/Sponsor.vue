<template>
    <section class="page-section">
      <b-container>
        <HeaderPage :title="sponsor.name" />
        <b-row>
          <b-col>
            <b-card class="overflow-hidden" style="max-width: 1540px;">
              <b-row>
                <b-col md="6" align="center">
                  <b-card-img :src="sponsor.photoUrl" class="rounded-0 mb-2"></b-card-img>
                </b-col>
                <b-col md="6">
                  <b-card-body :title="sponsor.name">
                    <b-card-text align="left">
                      <b>Animal Patrocinado:</b> {{ sponsor.animal }}
                    </b-card-text>
                    <b-card-text>
                      <b>Descrição:</b>
                      {{ sponsor.description }}
                    </b-card-text>
                    <b-card-text>
                      <b>Link:</b>
                      <a :href="sponsor.link" target="_blank">{{ sponsor.link }}</a>
                    </b-card-text>
                    <router-link
                      :to="{ name: 'sponsors' }"
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
    name: "Sponsor",
    components: {
      HeaderPage,
    },
    data() {
      return {
        sponsor: "",
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getSponsorsById"]),
    },
    created() {
      this.sponsor = this.getSponsorsById(this.$route.params.sponsorId);
    },
  };
  </script>
  