<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Editar Especialista" />

      <!-- FORM -->
      <b-row>
        <b-col cols="2"></b-col>
        <b-col cols="8">
          <form @submit.prevent="update">
            <div class="form-group">
              <input
                v-model="expert.name"
                type="text"
                class="form-control form-control-lg"
                id="txtName"
                placeholder="Escreve o nome"
                required
              />
            </div>
            <div class="form-group">
              <select id="sltAnimaliaFamily" class="form-control form-control-lg" v-model="expert.animaliaFamily">
                <option value>-- SELECIONA FAMÍLIA ANIMALIA --</option>
                <option value="mamífero">Mamífero</option>
                <option value="anfíbio">Anfíbio</option>
                <option value="ave">Ave</option>
                <option value="réptil">Réptil</option>
                <option value="peixe">Peixe</option>
              </select>
            </div>
            <div class="form-group">
              <textarea
                id="txtDescription"
                class="form-control form-control-lg"
                placeholder="Escreve a descrição"
                cols="30"
                rows="10"
                v-model="expert.description"
                required
              ></textarea>
            </div>
            <div class="form-group">
              <input
                v-model="expert.contactInfo"
                type="email"
                class="form-control form-control-lg"
                id="txtEmail"
                placeholder="Escreve o email de contato"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="expert.links[0].url"
                type="url"
                class="form-control form-control-lg"
                id="txtPhoto"
                placeholder="Escreve o link para a foto"
                required
              />
            </div>
            <button
              type="button"
              class="btn btn-outline-success btn-lg mr-2"
              @click="removeComments()"
            >
              <i class="fas fa-edit"></i> REMOVER COMENTÁRIOS
            </button>
            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-edit"></i> ATUALIZAR
            </button>
            <router-link
              :to="{ name: 'listExperts' }"
              tag="button"
              class="btn btn-outline-danger btn-lg"
            >
              <i class="fas fa-window-close"></i> CANCELAR
            </router-link>
          </form>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import { EDIT_EXPERT } from "@/store/experts/expert.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "EditExpert",
  components: {
    HeaderPage,
  },
  data: () => {
    return {
      expert: {},
    };
  },
  computed: {
    ...mapGetters("expert", ["getExpertById", "getMessage"]),
  },
  methods: {
    removeComments() {
      this.expert.comments = [];
      this.$alert(
        "Comentários removidos, clique em atualizar!",
        "Comentários!",
        "success"
      );
    },
    update() {
      this.$store.dispatch(`expert/${EDIT_EXPERT}`, this.expert).then(
        () => {
          this.$alert(this.getMessage, "Especialista atualizado!", "success");
          router.push({ name: "listExperts" });
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
  },
  created() {
    this.expert = this.getExpertById(this.$route.params.expertId);
  },
};
</script>

<style scoped>
.center_div {
  margin: 0 auto;
  width: 80%;
}
</style>
