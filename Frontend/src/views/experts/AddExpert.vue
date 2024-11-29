<template>
  <!-- Portfolio Section -->
  <section class="page-section">
    <b-container>
      <HeaderPage title="Adicionar Especialista" />

      <!-- FORM -->
      <b-row>
        <b-col cols="2"></b-col>
        <b-col>
          <form @submit.prevent="add">
            <div class="form-group">
              <input
                v-model="name"
                type="text"
                class="form-control form-control-lg"
                id="txtName"
                placeholder="Escreve o nome"
                required
              />
            </div>
            <div class="form-group">
              <select id="sltAnimaliaFamily" class="form-control form-control-lg" v-model="animaliaFamily" required>
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
                v-model="description"
                required
              ></textarea>
            </div>
            <div class="form-group">
              <input
                v-model="contactInfo"
                type="email"
                class="form-control form-control-lg"
                id="txtEmail"
                placeholder="Escreve o email de contato"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="links[0].url"
                type="url"
                class="form-control form-control-lg"
                id="txtPhoto"
                placeholder="Escreve o link para a foto"
                required
              />
            </div>
            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-plus-square"></i> ADICIONAR
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
import { ADD_EXPERT } from "@/store/experts/expert.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "AddExpert",
  components: {
    HeaderPage,
  },
  data: () => {
    return {
      name: "",
      animaliaFamily: "",
      description: "",
      contactInfo: "",
      links: [{ types: "photo", url: "" }],
    };
  },
  computed: {
    ...mapGetters("expert", ["getMessage"]),
  },
  methods: {
    add() {
      const payload = {
        name: this.name,
        animaliaFamily: this.animaliaFamily,
        description: this.description,
        contactInfo: this.contactInfo,
        links: this.links,
      };
      this.$store.dispatch(`expert/${ADD_EXPERT}`, payload).then(
        () => {
          this.$alert(this.getMessage, "Especialista adicionado!", "success");
          router.push({ name: "listExperts" });
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
  },
};
</script>
