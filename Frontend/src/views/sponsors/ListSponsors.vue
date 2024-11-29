<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Gestão de Patrocinadores" />

      <b-row class="mb-4">
        <b-col cols="2"></b-col>
        <b-col>
          <router-link
            :to="{ name: 'addSponsor' }"
            tag="button"
            class="btn btn-outline-success mr-2 mt-2"
          >
            <i class="fas fa-plus-square"></i> ADICIONAR PATROCINADOR
          </router-link>
          <router-link
            :to="{ name: 'admin' }"
            tag="button"
            class="btn btn-outline-info mr-2 mt-2"
          >
            <i class="fas fa-bars"></i> MENU PRINCIPAL
          </router-link>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>

      <b-row>
        <b-col cols="2"></b-col>
        <b-col>
          <table class="table table-striped">
            <thead class="thead-dark">
              <tr>
                <th>NOME</th>
                <th>ANIMAL</th>
                <th>AÇÕES</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="sponsor in sponsors" :key="sponsor._id">
                <td>{{ sponsor.name }}</td>
                <td>{{ sponsor.animal }}</td>
                <td>
                  <router-link
                    :to="{ name: 'editSponsor', params: { sponsorId: sponsor._id } }"
                    class="btn btn-outline-success mr-2 mt-2"
                  >
                    <i class="fas fa-edit"></i> EDITAR
                  </router-link>
                  <button
                    @click="remove(sponsor._id)"
                    class="btn btn-outline-danger mr-2 mt-2"
                  >
                    <i class="fas fa-trash-alt"></i> REMOVER
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import { FETCH_SPONSORS, REMOVE_SPONSOR } from "@/store/sponsors/sponsor.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import { mapGetters } from "vuex";

export default {
  name: "ListSponsors",
  components: { HeaderPage },
  data: () => ({}),
  computed: {
    ...mapGetters("sponsor", ["getSponsors", "getMessage"]),
  },
  methods: {
    fetch() {
      this.$store.dispatch(`sponsor/${FETCH_SPONSORS}`);
    },
    remove(id) {
      this.$store.dispatch(`sponsor/${REMOVE_SPONSOR}`, id).then(
        () => {
          this.$alert(this.getMessage, "Patrocinador removido!", "success");
          this.fetch();
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
  },
  created() {
    this.fetch();
  },
};
</script>
