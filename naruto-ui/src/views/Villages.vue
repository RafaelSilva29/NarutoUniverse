<template>
  <div id="villages">
    <v-container>
      <h1 class="mt-5">Villages</h1>
      <v-row class="justify-center" v-if="!loading">
        <v-col cols="12" sm="3" md="3">
          <v-text-field
            label="Search"
            append-icon="mdi-magnify"
            v-model="search"
          ></v-text-field>
        </v-col>
      </v-row>
      <div class="wrapper mt-2" v-if="!loading">
        <v-card
        transition="scale-transition"
        max-width="280"
        min-width="280"
        class="card-custom"
        v-for="village in villages" :key="village.image"
        @click.prevent="goTo(village)"
      >
        <v-img
          class="white--text align-end"
          height="200px"
          :src="village.image"
        >
          <v-card-title>{{ village.name }}</v-card-title>
        </v-img>
      </v-card>
      </div>
      <div v-else class="mt-12">
        <v-progress-circular
          indeterminate
          color="primary"
          :size="100"
        />
        <p class="mt-5">Loading, Please wait...</p>
      </div>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'Villages',
  data() {
    return {
      data: [],
      loading: true,
      search: '',
    }
  },
  async created() {
    await this.axios.get('/villages').then((response) => {
      this.data = response.data
      setTimeout(() => {
        this.loading = false
      }, 500);
    })
    .catch((error) => {
      alert(error)
    })
  },
  methods: {
    goTo(item) {
      this.$router.push({
          name: 'IndividualVillage', 
          params: { idVillage: item.id }
      });
    }
  },
  computed: {
    villages() {
      let resp = []
      if (this.search != '') {
        this.data.forEach(element => {
          const search = this.search.toLowerCase()
          const name = element.name.toLowerCase()
          if (name.includes(search)) {
            resp.push(element)
          }
        });
      } else {
        resp = this.data
      }
      return resp
    }
  },
}
</script>

<style lang="scss">
#villages {
  text-align: center;
}
  .wrapper {
    grid-template-rows: auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 15px;
    justify-items: center;
    align-items: center;
  }
  .card-custom {
    cursor: pointer;
  }
  .card-custom:hover {
    transform: scale(1.05);
  }

</style>
