<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Editar Patrocinador" />

      <b-row>
        <b-col cols="2"></b-col>
        <b-col cols="8">
          <form @submit.prevent="update">
            <div class="form-group">
              <input
                v-model="sponsor.name"
                type="text"
                class="form-control form-control-lg"
                placeholder="Escreve o nome"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="sponsor.animal"
                type="text"
                class="form-control form-control-lg"
                placeholder="Escreve o animal patrocinado"
                required
              />
            </div>
            <div class="form-group">
              <textarea
                v-model="sponsor.description"
                class="form-control form-control-lg"
                placeholder="Escreve a descrição do patrocinador"
                cols="30"
                rows="5"
                required
              ></textarea>
            </div>
            <div class="form-group">
              <input
                v-model="sponsor.link"
                type="url"
                class="form-control form-control-lg"
                placeholder="Escreve o link do site do patrocinador"
              />
            </div>
            <button type="submit" class="btn btn-outline-success btn-lg mr-2">
              <i class="fas fa-edit"></i> ATUALIZAR
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
import { EDIT_SPONSOR } from "@/store/sponsors/sponsor.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "@/router";
import { mapGetters } from "vuex";

export default {
  name: "EditSponsor",
  components: { HeaderPage },
  data: () => ({
    sponsor: {},
  }),
  computed: {
    ...mapGetters("sponsor", ["getSponsorById", "getMessage"]),
  },
  methods: {
    update() {
      this.$store.dispatch(`sponsor/${EDIT_SPONSOR}`, this.sponsor).then(
        () => {
          this.$alert(this.getMessage, "Patrocinador atualizado!", "success");
          router.push({ name: "listSponsors" });
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
  },
  created() {
    this.sponsor = this.getSponsorById(this.$route.params.sponsorId);
  },
};
</script>
