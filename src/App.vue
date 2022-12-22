<template>
  <article>
    <Table
      title="Étoiles les plus proches de la terre"
      :size="default_size"
      :array="near_stars"
    />

    <Table
      title="Étoiles les plus brillantes"
      :size="default_size"
      :array="bright_stars"
    />

    <Table
      title="Étoiles les plus chaudes"
      :size="default_size"
      :array="hot_stars"
    />
  </article>
</template>

<script>
import dataset from "@/assets/json/stars.json";
import Table from "@/components/Table.vue";

export default {
  name: "App",
  components: {
    Table,
  },
  data() {
    return {
      default_size: 50,
      near_stars: undefined,
      bright_stars: undefined,
      hot_stars: undefined,
    };
  },
  mounted() {
    this.near_stars = this.sortData("dist");
    this.bright_stars = this.sortData("lum", true);
    this.hot_stars = this.sortData("ci", true);
  },
  methods: {
    sortData(sortBy, desc = false) {
      let sort = [-1, 1];
      if (desc) sort = [1, -1];
      return dataset
        .sort((a, b) =>
          a[sortBy] < b[sortBy] ? sort[0] : a[sortBy] > b[sortBy] ? sort[1] : 0
        )
        .slice();
    },
  },
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 14px;
}

::-webkit-scrollbar-thumb {
  box-shadow: inset 0 0 14px 14px black;
  border: solid 4px transparent;
  border-radius: 14px;
}

::-webkit-scrollbar-button {
  display: none;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  scroll-behavior: smooth;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  overflow: overlay;
}

article {
  margin: 1.4rem;
  display: flex;
  flex-direction: column;
  gap: 2.4rem;
}
</style>
