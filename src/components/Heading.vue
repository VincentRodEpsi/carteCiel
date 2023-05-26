<template>
  <header>
    <section>
      <form @submit.prevent>
        <label for="name">Rechercher une étoile : </label>
        <input id="name" name="name" type="text" />
      </form>
    </section>
    <section>
      <form @submit.prevent>
        <div>
          <label for="near_nb">Étoiles distantes : </label>
          <input id="near_nb" v-model="near_nb" name="near_nb" type="range" />
          <span>{{ near_nb }}</span>
        </div>
        <div>
          <label for="bright_nb">Étoiles brillantes : </label>
          <input
            id="bright_nb"
            v-model="bright_nb"
            name="bright_nb"
            type="range"
          />
          <span>{{ bright_nb }}</span>
        </div>
        <div>
          <label for="hot_nb">Étoiles chaudes : </label>
          <input id="hot_nb" v-model="hot_nb" name="hot_nb" type="range" />
          <span>{{ hot_nb }}</span>
        </div>
      </form>
    </section>
  </header>
</template>

<script>
export default {
  name: "HeadingComponent",
  data() {
    return {
      near_nb: 0,
      bright_nb: 0,
      hot_nb: 0,
    };
  },
  watch: {
    near_nb(value) {
      this.$emit("near", value);
    },
    bright_nb(value) {
      this.$emit("bright", value);
    },
    hot_nb(value) {
      this.$emit("hot", value);
    },
  },
};
</script>

<style lang="scss" scoped>
header {
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  display: flex;
  flex-flow: row nowrap;
  gap: 1rem;
  justify-content: space-between;
  left: 0;
  padding: 1rem;
  pointer-events: all;
  position: absolute;
  right: 0;
  top: 0;
  z-index: 1;

  > section {
    > form {
      align-items: center;
      display: flex;
      flex-flow: row nowrap;
      gap: 1rem;
      justify-content: flex-start;

      input[type="text"] {
        border: 1px solid #ccc;
        border-radius: 4px;
        outline: none;
        padding: 0.5rem;
      }

      > div {
        align-items: center;
        display: flex;
        flex-flow: row nowrap;
        gap: 0.4rem;
        justify-content: flex-start;

        span {
          text-align: center;
          width: 20px;
        }
      }
    }

    &:last-of-type > form {
      gap: 2rem;
    }
  }
}
</style>
