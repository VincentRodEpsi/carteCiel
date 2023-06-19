<template>
  <!--<article>
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
  </article>-->

  <Popup v-if="star" :star="star" @close="resetStar" />
  <Heading @bright="setBright" @hot="setHot" @near="setNear" />
  <Map :dataset="array" @select="setStar" />
</template>

<script>
import dataset from "@/assets/json/stars.json";
//import Table from "@/components/Table.vue";
import Heading from "@/components/Heading.vue";
import Map from "@/components/Map.vue";
import Popup from "@/components/Popup.vue";

export default {
  name: "App",
  components: {
    //Table,
    Heading,
    Map,
    Popup,
  },
  computed: {
    array() {
      if (this.near_stars && this.bright_stars && this.hot_stars) {
        return this.removeDuplicatesById([
          ...this.near_stars,
          ...this.bright_stars,
          ...this.hot_stars,
        ]);
      }
      return [];
    },
  },
  data() {
    return {
      default_size: 50,
      near_stars: undefined,
      bright_stars: undefined,
      hot_stars: undefined,
      star: false,
    };
  },
  methods: {
    setBright(value) {
      this.bright_stars = this.sortData("lum", value, true);
    },
    setHot(value) {
      this.hot_stars = this.sortData("ci", value, true);
    },
    setNear(value) {
      this.near_stars = this.sortData("dist", value);
    },
    setStar(value) {
      console.log(value);
      this.star = value;
    },
    resetStar() {
      this.star = false;
    },
    sortData(sortBy, nb, desc = false) {
      let sort = [-1, 1];
      if (desc) sort = [1, -1];
      return dataset
        .sort((a, b) =>
          a[sortBy] < b[sortBy] ? sort[0] : a[sortBy] > b[sortBy] ? sort[1] : 0
        )
        .slice(0, nb);
    },
    removeDuplicatesById(arr) {
      const uniqueObjects = {};
      const result = [];

      for (let obj of arr) {
        const id = obj.id;
        if (!uniqueObjects[id]) {
          uniqueObjects[id] = true;
          result.push(obj);
        }
      }

      return result;
    },
  },
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 14px;
}

::-webkit-scrollbar-thumb {
  border: solid 4px transparent;
  border-radius: 14px;
  box-shadow: inset 0 0 14px 14px black;
}

::-webkit-scrollbar-button {
  display: none;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  overflow: hidden;
  scroll-behavior: smooth;
}

article {
  display: flex;
  flex-direction: column;
  gap: 2.4rem;
  margin: 1.4rem 1.4rem 2rem;
}
</style>
