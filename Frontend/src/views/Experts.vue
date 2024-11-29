<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Especialistas" />
  
        <!-- Filters -->
        <b-row>
          <div class="form-group mb-5">
            <input
              class="form-control form-control-lg"
              type="text"
              placeholder="Procura pelo nome..."
              v-model="filterName"
            />
          </div>
          <div class="form-group mx-sm-3 mb-5">
            <select class="form-control form-control-lg" v-model="filterAnimaliaFamily">
              <option value="todos">todas as famílias</option>
              <option value="mamífero">mamíferos</option>
              <option value="anfíbio">anfíbios</option>
              <option value="ave">aves</option>
              <option value="réptil">répteis</option>
              <option value="peixe">peixes</option>
            </select>
          </div>
          <div class="form-group mb-5">
            <button class="btn btn-success btn-lg" @click="sortExperts">
              <i :class="classSorter"></i> ordenar
            </button>
          </div>
        </b-row>
  
        <!-- Experts List -->
        <div class="row">
          <Expert v-for="e in filteredExperts" :key="e._id" :expert="e" />
        </div>
      </b-container>
    </section>
  </template>
  
  <script>
  import Expert from "@/components/Expert.vue";
  import HeaderPage from "@/components/HeaderPage.vue";
  import { FETCH_EXPERTS } from "@/store/experts/expert.constants";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Experts",
    components: {
      HeaderPage,
      Expert,
    },
    data() {
      return {
        filterName: "",
        filterAnimaliaFamily: "todos",
        reverse: false,
        experts: [],
      };
    },
    computed: {
      ...mapGetters("expert", ["getExperts"]),
      classSorter() {
        return {
          "fas fa-sort-alpha-up": !this.reverse,
          "fas fa-sort-alpha-down": this.reverse,
        };
      },
      filteredExperts() {
        return this.experts.filter((expert) => {
          let filterNameResult = true;
          let filterAnimaliaFamilyResult = true;
  
          if (this.filterName !== "") {
            filterNameResult = expert.name.includes(this.filterName);
          }
          if (this.filterAnimaliaFamily !== "todos") {
            filterAnimaliaFamilyResult = expert.animaliaFamily === this.filterAnimaliaFamily;
          }
          return filterNameResult && filterAnimaliaFamilyResult;
        });
      },
    },
    methods: {
      compareExpertNames(expert1, expert2) {
        if (!this.reverse) {
          if (expert1.name > expert2.name) return 1;
          if (expert1.name < expert2.name) return -1;
        } else {
          if (expert1.name < expert2.name) return 1;
          if (expert1.name > expert2.name) return -1;
        }
        return 0;
      },
      sortExperts() {
        this.experts.sort(this.compareExpertNames);
        this.reverse = !this.reverse;
      },
    },
    created() {
      this.$store.dispatch(`expert/${FETCH_EXPERTS}`).then(
        () => {
          this.experts = this.getExperts;
        },
        (err) => this.$alert(`${err.message}`, "Erro", "error")
      );
    },
  };
  </script>
  