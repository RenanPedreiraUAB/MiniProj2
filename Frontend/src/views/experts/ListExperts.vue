<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Gestão de Especialistas" />
      
      <!-- Top Menu -->
      <b-row class="mb-4">
        <b-col cols="2"></b-col>
        <b-col>
          <router-link
            :to="{ name: 'addExpert' }"
            tag="button"
            class="btn btn-outline-success mr-2 mt-2"
          >
            <i class="fas fa-plus-square"></i> ADICIONAR ESPECIALISTA
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

      <!-- Experts Table -->
      <b-row>
        <b-col cols="2"></b-col>
        <b-col>
          <table class="table table-striped">
            <thead class="thead-dark">
              <tr>
                <th scope="col">
                  NOME
                  <i class="fas fa-arrow-up" v-if="sortType === 1" @click="sort()" />
                  <i class="fas fa-arrow-down" v-else @click="sort()" />
                </th>
                <th scope="col">FAMÍLIA ANIMALIA</th>
                <th scope="col">AÇÕES</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="expert in experts" :key="expert._id">
                <td class="pt-4">{{ expert.name }}</td>
                <td class="pt-4">{{ expert.animaliaFamily }}</td>
                <td>
                  <router-link
                    :to="{ name: 'editExpert', params: { expertId: expert._id } }"
                    tag="button"
                    class="btn btn-outline-success mr-2 mt-2"
                  >
                    <i class="fas fa-edit"></i> EDITAR
                  </router-link>
                  <button
                    @click="viewExpert(expert._id)"
                    type="button"
                    class="btn btn-outline-success mr-2 mt-2"
                  >
                    <i class="fas fa-search"></i> VER
                  </button>
                  <button
                    @click="removeExpert(expert._id)"
                    type="button"
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
import { FETCH_EXPERTS, REMOVE_EXPERT } from "@/store/experts/expert.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import { mapGetters } from "vuex";

export default {
  name: "ListExperts",
  components: {
    HeaderPage,
  },
  data() {
    return {
      experts: [],
      sortType: 1,
    };
  },
  computed: {
    ...mapGetters("expert", ["getExperts", "getMessage"]),
  },
  methods: {
    fetchExperts() {
      this.$store.dispatch(`expert/${FETCH_EXPERTS}`).then(
        () => {
          this.experts = this.getExperts;
        },
        (err) => {
          this.$alert(`${err.message}`, "Erro", "error");
        }
      );
    },
    sort() {
      this.experts.sort(this.compareNames);
      this.sortType *= -1;
    },
    compareNames(e1, e2) {
      if (e1.name > e2.name) return 1 * this.sortType;
      if (e1.name < e2.name) return -1 * this.sortType;
      return 0;
    },
    viewExpert(id) {
      const expert = this.experts.find((expert) => expert._id === id);
      this.$fire({
        title: expert.name,
        html: this.generateTemplate(expert),
        imageUrl: expert.image || "https://via.placeholder.com/150",
        imageWidth: 400,
        imageHeight: 200,
        imageAlt: "Imagem do Especialista",
      });
    },
    generateTemplate(expert) {
      return `
        <h4>Família Animalia: ${expert.animaliaFamily}</h4>
        <p>${expert.description || "Sem descrição disponível."}</p>
      `;
    },
    removeExpert(id) {
      this.$confirm(
        "Se sim, clique em OK",
        "Deseja mesmo remover o especialista?",
        "warning",
        { confirmButtonText: "OK", cancelButtonText: "Cancelar" }
      ).then(
        () => {
          this.$store.dispatch(`expert/${REMOVE_EXPERT}`, id).then(() => {
            this.$alert(this.getMessage, "Especialista removido!", "success");
            this.fetchExperts();
          });
        },
        () => {
          this.$alert("Remoção cancelada!", "Informação", "info");
        }
      );
    },
  },
  created() {
    this.fetchExperts();
  },
};
</script>
