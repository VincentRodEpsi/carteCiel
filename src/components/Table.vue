<template>
  <section>
    <h1>{{ title }}</h1>
    <form>
      <label for="near_nb">Nombre : </label>
      <input
        type="number"
        name="near_nb"
        id="near_nb"
        v-model="slice"
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
          <tr v-for="(star, index) of slice_array" :key="index">
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
</template>

<script>
export default {
  name: "TableComponent",
  props: ["title", "size", "array"],
  data() {
    return {
      slice: this.size,
      slice_array: null,
      raw_array: null,
    };
  },
  methods: {
    sliceArray() {
      this.slice_array = this.raw_array.slice(0, this.slice);
    },
  },
  watch: {
    slice() {
      this.sliceArray();
    },
    array() {
      this.raw_array = this.array;
      this.sliceArray();
    },
  },
};
</script>

<style lang="scss" scoped>
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
</style>
