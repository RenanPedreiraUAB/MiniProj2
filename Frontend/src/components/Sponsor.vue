<template>
    <b-col cols="4">
      <b-card
        :title="sponsor.name"
        :img-src="sponsor.photoUrl"
        img-alt="Imagem do Patrocinador"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="mb-5"
      >
        <b-card-text>
          <kbd>{{ sponsor.animal }}</kbd>
        </b-card-text>
        <b-card-text>{{ sponsor.description }}</b-card-text>
        <b-card-text align="left">
          <i class="fas fa-star fa-lg" :style="{ color }" @click="evaluate()"></i>
          {{ sponsor.evaluation.length }}
        </b-card-text>
  
        <router-link
          :to="{ name: 'sponsor', params: { sponsorId: sponsor._id } }"
          tag="b-button"
          variant="outline-success"
          align="center"
          class="mr-2"
        >
          <i class="fas fa-search"></i> Ver Patrocinador
        </router-link>
        <b-button
          variant="info"
          :href="'https://www.facebook.com/sharer/sharer.php?u=' + sponsor.photoUrl"
          target="_blank"
        >
          <i class="fab fa-facebook"></i> Partilhar
        </b-button>
      </b-card>
    </b-col>
  </template>
  
  <script>
  import { EDIT_SPONSOR } from "@/store/sponsors/sponsor.constants";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Sponsor",
    props: ["sponsor"],
    data: function () {
      return {
        color: "",
      };
    },
    methods: {
      ...mapGetters("auth", ["getProfile"]),
      evaluate() {
        if (!this.sponsor.evaluation.includes(this.getProfile()._id)) {
          this.sponsor.evaluation.push(this.getProfile()._id);
          this.color = "red";
        } else {
          this.sponsor.evaluation = this.sponsor.evaluation.filter(
            (user) => user !== this.getProfile()._id
          );
          this.color = "black";
        }
        this.$store.dispatch(`sponsor/${EDIT_SPONSOR}`, this.sponsor).then(
          () => {
            this.$alert(
              `Obrigado por gostar do patrocinador ${this.sponsor.name}!`,
              "Gosto",
              "success"
            );
          },
          (err) => {
            this.$alert(`${err.message}`, "Erro", "error");
          }
        );
      },
    },
    created() {
      if (!this.sponsor.evaluation.includes(this.getProfile()._id)) {
        this.color = "black";
      } else {
        this.color = "red";
      }
    },
  };
  </script>
  