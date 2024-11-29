<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Adicionar Patrocinador" />

      <b-row>
        <b-col cols="2"></b-col>
        <b-col>
          <form @submit.prevent="add">
            <div class="form-group">
              <input
                v-model="name"
                type="text"
                class="form-control form-control-lg"
                placeholder="Escreve o nome"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="animal"
                type="text"
                class="form-control form-control-lg"
                placeholder="Escreve o animal patrocinado"
                required
              />
            </div>
            <div class="form-group">
              <textarea
                v-model="description"
                class="form-control form-control-lg"
                placeholder="Escreve a descrição do patrocinador"
                cols="30"
                rows="5"
                required
              ></textarea>
            </div>
            <div class="form-group">
              <input
                v-model="link"
                type="url"
                class="form-control form-control-lg"
                placeholder="Escreve o link do site do patrocinador"
              />
            </div>
            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-plus-square"></i> ADICIONAR
            </button>
            <router-link
              :to="{ name: 'listSponsors' }"
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
import { ADD_SPONSOR } from "@/store/sponsors/sponsor.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "AddSponsor",
  components: { HeaderPage },
  data: () => ({
    name: "",
    animal: "",
    description: "",
    link: "",
  }),
  computed: {
    ...mapGetters("sponsor", ["getMessage"]),
  },
  methods: {
    add() {
      const payload = {
        name: this.name,
        animal: this.animal,
        description: this.description,
        link: this.link,
      };
      this.$store.dispatch(`sponsor/${ADD_SPONSOR}`, payload).then(
        () => {
          this.$alert(this.getMessage, "Patrocinador adicionado!", "success");
          router.push({ name: "listSponsors" });
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
  },
};
</script>
