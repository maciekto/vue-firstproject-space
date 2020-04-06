<template>
      <input
        id="search"
        name="search"
        :class="{ dark: dark }"
        :value="value"
        @input="handleChange"
      />
</template>

<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>

<style lang="scss" scoped>
  input{
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    width: 250px;
    font-size: 18px;
    font-weight: 300;
    background: none;
    outline: none;
    height: 30px;
    border:0;
    border-bottom: 1px solid black;
    text-align: center;
    transition: box-shadow .5s ease-out;

    @media (min-width: 1024){
      font-weight: 600;
    }
  }
  input:focus{
    background: none;
    box-shadow: 0 10px 20px -8px rgba($color: #000000, $alpha: .5);
  }

  .dark{
    color: black;
    border-bottom-color: black;
  }
  .dark:focus{
    box-shadow: 0 10px 20px -8px rgba($color: #000000, $alpha: .5);
  }
</style>
