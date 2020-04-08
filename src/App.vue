
<template>
  <div :class="[{ flexStart: step === 1}, 'wrapper']">
    <transition name="fade">
      <HeroImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1"/>
    <div class="results" v-if="results && !loading && step === 1">
      <!-- eslint-disable-next-line -->
      <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)" />
    </div>
    <div class="loader" v-if="step === 1 && loading" />
    <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false" />
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from './components/Claim.vue';
import SearchInput from './components/SearchInput.vue';
import HeroImage from './components/HeroImage.vue';
import Item from './components/Item.vue';
import Modal from './components/Modal.vue';

const API = 'https://images-api.nasa.gov/search';

export default {
  name: 'App',
  data() {
    return {
      modalOpen: false,
      modalItem: null,
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  components: {
    Claim,
    SearchInput,
    HeroImage,
    Item,
    Modal,
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },
    // eslint-disable-next-line
    handleInput: debounce(function () {
      this.loading = true;
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss">

  *{
    box-sizing: border-box;
  }
  body{
    font-family: 'Montserrat', sans-serif;
    margin: 0;
    padding: 0;
  }

  .fade-enter-active, .fade-leave-active{
    transition: opacity .3s ease;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

  .wrapper{
    width: 100%;
    height: 100vh;
    min-height: 100vh;
    margin: 0;
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    &.flexStart {
      justify-content: flex-start;
    }
  }
  .loader {
    margin-top: 100px;
    display: inline-block;
    width: 80px;
    height: 80px;

    @media (min-width: 768px){
      width: 90px;
      height: 90px;
    }
  }
  .loader:after {
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid black;
    border-color: black transparent black transparent;
    animation: loading 1.2s linear infinite;
  }
  @keyframes loading {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  .results {
    margin-top: 50px;
    display: grid;
      grid-template-columns: 1fr 1fr;
    grid-gap: 10px;

    @media (min-width: 768px){
      width: 90vw;
      grid-template-columns: 1fr 1fr 1fr;
    }
  }
</style>
