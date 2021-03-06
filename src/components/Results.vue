<template>
  <div class="autocomplete">
    <form class="autocomplete__form" @submit="addImage">
      <div class="input-container">
        <span class="input-container__icon">
          <Icon fill="#fff"><IconMovie /></Icon>
        </span>
        <div class="search-container">
          <span class="search-container__icon">
            <Icon width="20" height="19"><IconMovie /></Icon>
          </span>
          <input
            class="search-input"
            type="text"
            name="search"
            autocomplete="off"
            v-model="search"
            @input="
              loadData();
              showResults();
            "
            @focus="
              changeLayout();
            "
            @blur="returnLayout"
          />
          <span class="input-placeholder">Enter movie name</span>
        </div>
      </div>
      <button type="submit" class="autocomplete__submit">
        <Icon fill="#f97838" viewBox="0 0 92 92" width="32" height="32"
          ><IconSearch
        /></Icon>
      </button>
    </form>
  </div>
  <ul class="results">
    <li class="result loader">Loading...</li>
    <li
      class="result"
      v-for="(result, i) in resultsMax"
      :key="i"
      @click="selectOption(result.title)"
    >
      <h3 class="result__title">{{ result.title }}</h3>
      <h4 class="result__about">
        {{ result.vote_average }} Rating, {{ parseInt(result.release_date) }}
      </h4>
    </li>
  </ul>
</template>

<script>
import Icon from "./Icon.vue";
import IconMovie from "./icons/IconMovie.vue";
import IconSearch from "./icons/IconSearch.vue";

export default {
  data() {
    return {
      results: [],
      search: "",
      limit: 8,
      minChar: 3,
      input: "",
      rect: "",
      form: "",
      icon: "",
      placeholder: "",
      resultsContainer: "",
      resultItems: "",
    };
  },
  methods: {
    loadData: function () {
      //fetching data from api if search length is more than 3
      if (this.search.length >= this.minChar) {
        fetch(
          `https://api.themoviedb.org/3/search/movie?api_key=2b7e82df2c1987e77309eccb59631802&language=en-US&query=${this.search}`
        )
          .then((response) => response.json())
          .then((json) => {
            document.querySelector(".loader").style.display = "none";
            this.results = json.results;
            console.log(this.resultsMax)
          });
      } else {
        this.results = [];
      }
    },
    variables() {
      //defining variables seperately to be used in later methods
      this.input = document.querySelector("input.search-input");
      this.rect = this.input.getBoundingClientRect();
      this.form = document.querySelector(".autocomplete__form");
      this.icon = document.querySelector(".search-container__icon");
      this.placeholder = document.querySelector(".input-placeholder");
    },
    showResults() {
      //show results container
      if (this.results.length) {
        this.resultsContainer.classList.add("results--show");
      } else {
        this.resultsContainer.classList.remove("results--show");
      }
    },
    selectOption(result) {
      //change input value to selected result
      this.search = result;
      this.results = [];
      this.placeholder.classList.add("hidden");
    },
    changeLayout() {
      //change layout of input element
      this.placeholder.classList.remove("hidden");
      this.placeholder.classList.add("input-placeholder--small");
      this.input.classList.add("expanded");
      this.input.style.width =
        this.form.offsetWidth -
        document.querySelector(".input-container__icon").offsetWidth +
        "px";
      this.input.style.top = this.rect.top + "px";
      this.input.style.left = this.rect.left + "px";
      this.icon.classList.add("search-container__icon--show");
      this.resultsLayout();
    },
    returnLayout() {
      //input layout return to initial
      this.input.classList.remove("expanded");
      this.input.style.width = "100%";
      this.input.style.top = "0";
      this.input.style.left = "0";
      this.icon.classList.remove("search-container__icon--show");

      if (!this.input.value) {
        this.placeholder.classList.remove("input-placeholder--small");
      } else {
        this.placeholder.classList.add("hidden");
      }

      this.resultsContainer.classList.remove("results--show");
    },
    resultsLayout() {
      //change results container layout
      this.resultsContainer = document.querySelector(".results");
      this.resultItems = document.querySelectorAll(".result");

      this.resultsContainer.style.top =
        this.input.getBoundingClientRect().top + this.input.scrollHeight + "px";
      this.resultsContainer.style.left = this.rect.left + "px";
      this.resultsContainer.style.width = this.input.offsetWidth + "px";
    },
  },
  computed: {
    resultsMax() {
      //show limited amount of titles
      return this.limit ? this.results.slice(0, this.limit) : this.results;
    },
  },
  mounted() {
    //initialize variables
    this.variables();
  },
  components: {
    Icon,
    IconMovie,
    IconSearch,
  },
};
</script>

<style lang="scss">
$orange: #f97838;
$red: #e23827;
$white: #fff;
$dark-gray: #19232c;
$light-gray: rgb(194, 194, 194);
$mid-gray: rgb(109, 109, 109);

.autocomplete {
  background: linear-gradient(90deg, $orange, $red);
  padding: 25px;

  &__form {
    display: flex;
    height: 55px;
  }

  &__submit {
    width: 55px;
    appearance: none;
    border: none;
    outline: none;
    margin-left: 5px;
    background-color: $white;
    border-radius: 3px;
    cursor: pointer;
  }
}

.input-container {
  display: flex;
  background-color: rgba(255, 255, 255, 0.3);
  width: 100%;
  position: relative;
  border-radius: 3px;

  &__icon {
    padding: 15px 10px;
    width: 47px;
  }
}

input.search-input {
  appearance: none;
  width: 100%;
  background-color: transparent;
  border: none;
  color: $white;
  padding: 20px 10px;
  font-weight: 600;
  transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out,
    padding-left 0.3s ease-in-out;
  position: relative;

  &:focus {
    border: none;
    background-color: $white;
    outline: none;
    color: $dark-gray;
  }

  &.expanded {
    position: fixed;
    z-index: 1;
    height: 65px;
    margin-top: -10px;
    border-radius: 3px 3px 0 0;
    padding-left: 57px;
  }
}

.input-placeholder {
  position: absolute;
  left: 0;
  top: 50%;
  z-index: 1;
  transform: translateY(-50%);
  color: $white;
  font-weight: 600;
  pointer-events: none;
  transition: top 0.3s ease-in-out, font-size 0.3s ease-in-out,
    color 0.3s ease-in-out, left 0.3s ease-in-out;

  &--small {
    top: 80%;
    font-size: 0.75rem;
    color: $light-gray;
    left: 57px;
  }
}

.search-container {
  width: 100%;
  display: flex;
  position: relative;

  &__icon {
    top: 50%;
    transform: translateY(-50%);
    max-width: 0;
    overflow: hidden;
    position: absolute;
    z-index: 2;
    transition: max-width 0.3s ease-in-out, opacity 0.3s ease-in-out;
    margin-left: 20px;
    margin-right: 15px;
    opacity: 0;

    &--show {
      max-width: 30px;
      opacity: 1;
    }
  }
}

ul.results {
  position: fixed;
  background-color: $white;
  z-index: 3;
  width: 100%;
  margin: 0;
  overflow: hidden;
  max-height: 0;
  transition: max-height 0.3s ease-in-out;
  padding-left: 57px;
  box-shadow: 0px 7px 7px rgba(0, 0, 0, 0.3);
  list-style: none;
  text-align: left;

  &--show {
    border-top: 1px solid $light-gray;
    max-height: 100vh;
  }

  .result {
    color: $mid-gray;
    cursor: pointer;
    padding: 20px 0 15px;

    &__title {
      font-size: 1rem;
      margin: 0;
    }

    &__about {
      font-size: 0.75rem;
      margin: 0;
    }
  }
}
</style>