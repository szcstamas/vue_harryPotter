<template>
  <div class="character-container" v-if="char">
    {{ char }}
    <router-link to="/harry-potter">⬅️ Back to all wizards</router-link>
    <h4>This is the page of</h4>
    <div class="flex-space-between character-name-box">
      <h1 class="character-name">{{ char.name }}</h1>
      <p class="character-house">{{ char.house }}</p>
    </div>
    <i>Played by {{ char.actor }}</i>
    <ul>
      <li>
        {{ characterGender }}
        {{ char.wizard ? "is a wizard" : "is not a wizard" }}
      </li>
      <li>
        {{ characterGender }}
        {{ stillAHogwartsStudent }}
      </li>
      <li>
        {{ characterGenderHas }} wand was created from {{ char.wand.wood }} and
        {{ char.wand.core }}
      </li>
      <li>{{ characterGenderHas }} patronus is {{ char.patronus }}</li>
    </ul>
    <div class="character-image-box">
      <img
        :src="char.image"
        :alt="char.name + 'is the character on the picture'"
      />
    </div>
  </div>
  <div v-else>
    <h1>Page not found (404)</h1>
    <p>Try a different page</p>
  </div>
</template>

<script>
import HarryPotterService from "@/services/HarryPotterService.js";

export default {
  props: ["id"],
  data() {
    return {
      char: null,
    };
  },
  created() {
    HarryPotterService.getCharacters()
      .then((res) => {
        return (this.char = res.data.find((char) => char.id === this.id));
      })
      .catch((error) => {
        console.table([error.code, error.message]);
      });
  },
  computed: {
    characterGender() {
      return this.char.gender === "male" ? "He" : "She";
    },
    characterGenderHas() {
      return this.char.gender === "male" ? "His" : "Her";
    },
    stillAHogwartsStudent() {
      return this.char.hogwartsStudent
        ? "is still a student"
        : "is not a student";
    },
  },
};
</script>

<style lang="scss" scoped>
.character-container {
  position: relative;
  margin-top: 2rem;
  padding: 4rem;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.1);

  h4 {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
    margin: 2rem 0 1rem 0;
  }
  .character-name-box {
    margin-bottom: 4rem;
  }
  .character-name {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 48px;
    font-weight: bold;
    line-height: 55px;
  }
  .character-house {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 18px;
    color: #ccc;
    text-transform: uppercase;
  }
  ul {
    padding-left: 2rem;
    width: 50%;

    li {
      margin: 1rem 0;
    }
  }
  .character-image-box {
    position: absolute;
    width: 250px;
    height: 300px;
    bottom: 0;
    right: 0;
    border-top-left-radius: 50%;
    opacity: 0.5;
    overflow: hidden;
    filter: grayscale(1);

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
}
</style>