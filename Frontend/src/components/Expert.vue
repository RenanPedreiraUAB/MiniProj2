<template>
    <b-col cols="4">
      <b-card
        :title="expert.name"
        :img-src="expert.photoUrl"
        img-alt="Imagem do Especialista"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="mb-5"
      >
        <b-card-text>
          <kbd>{{ expert.animaliaFamily }}</kbd>
        </b-card-text>
        <b-card-text>{{ expert.description }}</b-card-text>
        <b-card-text align="left">
          <i class="fas fa-star fa-lg" :style="{ color }" @click="evaluate()"></i>
          {{ expert.evaluation.length }}
        </b-card-text>
  
        <router-link
          :to="{ name: 'expert', params: { expertId: expert._id } }"
          tag="b-button"
          variant="outline-success"
          align="center"
          class="mr-2"
        >
          <i class="fas fa-search"></i> Ver Especialista
        </router-link>
        <b-button
          variant="info"
          :href="'https://www.facebook.com/sharer/sharer.php?u=' + expert.photoUrl"
          target="_blank"
        >
          <i class="fab fa-facebook"></i> Partilhar
        </b-button>
      </b-card>
    </b-col>
  </template>
  
  <script>
  import { EDIT_EXPERT } from "@/store/experts/expert.constants";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Expert",
    props: ["expert"],
    data: function () {
      return {
        color: "",
      };
    },
    methods: {
      ...mapGetters("auth", ["getProfile"]),
      evaluate() {
        if (!this.expert.evaluation.includes(this.getProfile()._id)) {
          this.expert.evaluation.push(this.getProfile()._id);
          this.color = "red";
        } else {
          this.expert.evaluation = this.expert.evaluation.filter(
            (user) => user !== this.getProfile()._id
          );
          this.color = "black";
        }
        this.$store.dispatch(`expert/${EDIT_EXPERT}`, this.expert).then(
          () => {
            this.$alert(
              `Obrigado por gostar do especialista ${this.expert.name}!`,
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
      if (!this.expert.evaluation.includes(this.getProfile()._id)) {
        this.color = "black";
      } else {
        this.color = "red";
      }
    },
  };
  </script>
  