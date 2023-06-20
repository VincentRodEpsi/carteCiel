<template>
  <article>
    <section>
      <h1>Données de l'étoile</h1>
      <div>
        <p>Nom : {{ name }}</p>
        <p>Distance : {{ star.dist }} parsecs</p>
        <p>Magnitude : {{ star.mag }}</p>
        <p>Luminosité : {{ star.lum }}</p>
        <p>Indice de couleur : {{ star.ci }}</p>
        <p>Constellation : {{ star.con }}</p>

        <h2>Position :</h2>
        <div>
          <p>X : {{ star.x }}</p>
          <p>Y : {{ star.y }}</p>
          <p>Z : {{ star.z }}</p>
        </div>
      </div>
      <div>
        <button type="button" @click.prevent="$emit('close')">Fermer</button>
      </div>
    </section>
  </article>
</template>

<script>
export default {
  name: "PopupComponent",
  props: {
    star: {
      type: Object,
      required: true,
    },
  },
  computed: {
    name() {
      if (this.star.bf) return this.star.bf;
      else if (this.star.poper) return this.star.poper;
      else return "Inconnu";
    },
  },
};
</script>

<style lang="scss" scoped>
article {
  align-items: center;
  backdrop-filter: blur(4px);
  background-color: rgba(0, 0, 0, 0.5);
  bottom: 0;
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  left: 0;
  margin: 0;
  position: absolute;
  right: 0;
  top: 0;
  z-index: 50;

  section {
    background-color: white;
    border-radius: 8px;
    max-width: 600px;
    padding: 1.5rem;

    h1 {
      font-size: 1.2em;
      font-weight: bold;
      text-align: center;
    }

    > div {
      margin-top: 1rem;

      > p {
        margin-bottom: 0.2rem;
      }

      h2 {
        font-size: 1em;
        font-weight: bold;
        margin-bottom: 0.2rem;
        margin-top: 1rem;
      }

      div {
        align-items: center;
        display: flex;
        flex-flow: row nowrap;
        gap: 2rem;
        justify-content: flex-start;
      }
    }

    > div:last-of-type {
      align-items: center;
      display: flex;
      flex-flow: row nowrap;
      justify-content: center;
      margin-top: 1.4rem;

      button {
        border: 1px solid black;
        border-radius: 8px;
        padding: 0.6rem 1.4rem;
        transition: all 120ms;

        &:hover {
          background-color: #ddd;
        }
      }
    }
  }
}
</style>
