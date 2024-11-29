<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Patrocinadores" />
  
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
          <div class="form-group mb-5 mx-sm-3">
            <button class="btn btn-success btn-lg" @click="sortSponsors">
              <i :class="classSorter"></i> ordenar
            </button>
          </div>
        </b-row>
  
        <!-- Sponsors List -->
        <div class="row">
          <Sponsor v-for="s in filteredSponsors" :key="s._id" :sponsor="s" />
        </div>
      </b-container>
    </section>
  </template>
  
  <script>
  import Sponsor from "@/components/Sponsor.vue";
  import HeaderPage from "@/components/HeaderPage.vue";
  import { FETCH_SPONSORS } from "@/store/sponsors/sponsor.constants";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Sponsors",
    components: {
      HeaderPage,
      Sponsor,
    },
    data() {
      return {
        filterName: "",
        reverse: false,
        sponsors: [],
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getSponsors"]),
      classSorter() {
        return {
          "fas fa-sort-alpha-up": !this.reverse,
          "fas fa-sort-alpha-down": this.reverse,
        };
      },
      filteredSponsors() {
        return this.sponsors.filter((sponsor) => {
          let filterNameResult = true;
          if (this.filterName !== "") {
            filterNameResult = sponsor.name.includes(this.filterName);
          }
          return filterNameResult;
        });
      },
    },
    methods: {
      compareSponsorNames(s1, s2) {
        if (!this.reverse) {
          if (s1.name > s2.name) return 1;
          if (s1.name < s2.name) return -1;
        } else {
          if (s1.name < s2.name) return 1;
          if (s1.name > s2.name) return -1;
        }
        return 0;
      },
      sortSponsors() {
        this.sponsors.sort(this.compareSponsorNames);
        this.reverse = !this.reverse;
      },
    },
    created() {
      this.$store.dispatch(`sponsor/${FETCH_SPONSORS}`).then(
        () => {
          this.sponsors = this.getSponsors;
        },
        (err) => this.$alert(`${err.message}`, "Erro", "error")
      );
    },
  };
  </script>
  