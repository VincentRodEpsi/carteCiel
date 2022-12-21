<template>
  <article>
    <section>
      <h1>Étoiles les plus proches de la terre</h1>
      <form>
        <label for="near_nb">Nombre : </label>
        <input
          type="number"
          name="near_nb"
          id="near_nb"
          v-model="near_stars_nb"
          min="0"
          required
        />
      </form>
      <div>
        <table>
          <thead>
            <tr>
              <td>id</td>
              <td>name</td>
              <td>ra</td>
              <td>dec</td>
              <td>dist</td>
              <td>pmra</td>
              <td>pmdec</td>
              <td>rv</td>
              <td>mag</td>
              <td>ci</td>
              <td>x</td>
              <td>y</td>
              <td>z</td>
              <td>con</td>
              <td>lum</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(star, index) of near_stars.list" :key="index">
              <td>{{ star.id }}</td>
              <td>{{ star.name }}</td>
              <td>{{ star.ra }}</td>
              <td>{{ star.dec }}</td>
              <td>{{ star.dist }}</td>
              <td>{{ star.pmra }}</td>
              <td>{{ star.pmdec }}</td>
              <td>{{ star.rv }}</td>
              <td>{{ star.mag }}</td>
              <td>{{ star.ci }}</td>
              <td>{{ star.x }}</td>
              <td>{{ star.y }}</td>
              <td>{{ star.z }}</td>
              <td>{{ star.con }}</td>
              <td>{{ star.lum }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section>
      <h1>Étoiles les plus brillantes</h1>
      <form>
        <label for="bright_nb">Nombre : </label>
        <input
          type="number"
          name="bright_nb"
          id="bright_nb"
          v-model="bright_stars_nb"
          min="0"
          required
        />
      </form>
      <div>
        <table>
          <thead>
            <tr>
              <td>id</td>
              <td>name</td>
              <td>ra</td>
              <td>dec</td>
              <td>dist</td>
              <td>pmra</td>
              <td>pmdec</td>
              <td>rv</td>
              <td>mag</td>
              <td>ci</td>
              <td>x</td>
              <td>y</td>
              <td>z</td>
              <td>con</td>
              <td>lum</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(star, index) of bright_stars.list" :key="index">
              <td>{{ star.id }}</td>
              <td>{{ star.name }}</td>
              <td>{{ star.ra }}</td>
              <td>{{ star.dec }}</td>
              <td>{{ star.dist }}</td>
              <td>{{ star.pmra }}</td>
              <td>{{ star.pmdec }}</td>
              <td>{{ star.rv }}</td>
              <td>{{ star.mag }}</td>
              <td>{{ star.ci }}</td>
              <td>{{ star.x }}</td>
              <td>{{ star.y }}</td>
              <td>{{ star.z }}</td>
              <td>{{ star.con }}</td>
              <td>{{ star.lum }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section>
      <h1>Étoiles les plus chaudes</h1>
      <form>
        <label for="hot_nb">Nombre : </label>
        <input
          type="number"
          name="hot_nb"
          id="hot_nb"
          v-model="hot_stars_nb"
          min="0"
          required
        />
      </form>
      <div>
        <table>
          <thead>
            <tr>
              <td>id</td>
              <td>name</td>
              <td>ra</td>
              <td>dec</td>
              <td>dist</td>
              <td>pmra</td>
              <td>pmdec</td>
              <td>rv</td>
              <td>mag</td>
              <td>ci</td>
              <td>x</td>
              <td>y</td>
              <td>z</td>
              <td>con</td>
              <td>lum</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(star, index) of hot_stars.list" :key="index">
              <td>{{ star.id }}</td>
              <td>{{ star.name }}</td>
              <td>{{ star.ra }}</td>
              <td>{{ star.dec }}</td>
              <td>{{ star.dist }}</td>
              <td>{{ star.pmra }}</td>
              <td>{{ star.pmdec }}</td>
              <td>{{ star.rv }}</td>
              <td>{{ star.mag }}</td>
              <td>{{ star.ci }}</td>
              <td>{{ star.x }}</td>
              <td>{{ star.y }}</td>
              <td>{{ star.z }}</td>
              <td>{{ star.con }}</td>
              <td>{{ star.lum }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </article>
</template>

<script>
import dataset from "@/assets/json/stars.json";

export default {
  name: "App",
  data() {
    return {
      near_stars_nb: 50,
      near_stars: {
        raw_list: null,
        list: null,
      },
      bright_stars_nb: 50,
      bright_stars: {
        raw_list: null,
        list: null,
      },
      hot_stars_nb: 50,
      hot_stars: {
        raw_list: null,
        list: null,
      },
    };
  },
  mounted() {
    this.near_stars.raw_list = this.sortData("dist");
    this.bright_stars.raw_list = this.sortData("lum", true);
    this.hot_stars.raw_list = this.sortData("ci", true);

    this.computeNearStars();
    this.computeBrightStars();
    this.computeHotStars();
  },
  methods: {
    computeNearStars() {
      this.near_stars.list = this.near_stars.raw_list.slice(
        0,
        this.near_stars_nb
      );
    },
    computeBrightStars() {
      this.bright_stars.list = this.bright_stars.raw_list.slice(
        0,
        this.bright_stars_nb
      );
    },
    computeHotStars() {
      this.hot_stars.list = this.hot_stars.raw_list.slice(0, this.hot_stars_nb);
    },
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
  watch: {
    near_stars_nb() {
      this.computeNearStars();
    },
    bright_stars_nb() {
      this.computeBrightStars();
    },
    hot_stars_nb() {
      this.computeHotStars();
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

  section {
    h1 {
      margin-bottom: 6px;
      font-size: 1.6em;
    }

    form {
      margin-bottom: 1rem;
    }

    div {
      max-height: 300px;
      overflow: overlay;
      border-radius: 6px;
      -webkit-box-shadow: 0 5px 15px 2px rgba(0, 0, 0, 0.24);
      box-shadow: 0 5px 15px 2px rgba(0, 0, 0, 0.24);

      table {
        width: 100%;
        border-collapse: collapse;

        thead {
          position: sticky;
          top: 0;
          color: white;
          font-weight: bold;
          background-color: #2c3e50;
        }

        tr:nth-of-type(even) {
          background-color: rgba(0, 0, 0, 0.1);
        }

        td {
          padding: 0.4rem 0.6rem;
        }
      }
    }
  }
}
</style>
