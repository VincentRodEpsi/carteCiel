<template>
  <header>
    <section>
      <form @submit.prevent>
        <div>
          <label for="near_nb">Étoiles distantes : </label>
          <input
            id="near_nb"
            v-model="near_nb"
            max="1000"
            min="0"
            name="near_nb"
            step="1"
            type="range"
          />
          <span>{{ near_nb }}</span>
        </div>
        <div>
          <label for="bright_nb">Étoiles brillantes : </label>
          <input
            id="bright_nb"
            v-model="bright_nb"
            max="1000"
            min="0"
            name="bright_nb"
            step="1"
            type="range"
          />
          <span>{{ bright_nb }}</span>
        </div>
        <div>
          <label for="hot_nb">Étoiles chaudes : </label>
          <input
            id="hot_nb"
            v-model="hot_nb"
            max="1000"
            min="0"
            name="hot_nb"
            step="1"
            type="range"
          />
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
      near_nb: 100,
      bright_nb: 100,
      hot_nb: 100,
    };
  },
  watch: {
    near_nb: {
      handler(value) {
        this.$emit("near", value);
      },
      immediate: true,
    },
    bright_nb: {
      handler(value) {
        this.$emit("bright", value);
      },
      immediate: true,
    },
    hot_nb: {
      handler(value) {
        this.$emit("hot", value);
      },
      immediate: true,
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
  justify-content: center;
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
      gap: 6rem;
      justify-content: center;
      width: 100%;

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
  }
}
</style>
