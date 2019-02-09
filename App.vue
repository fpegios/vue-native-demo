<template>
  <view class="container">
    <text class="text text-color-primary">{{ message }}</text>
    <image class="image" :source="require('./assets/icon.png')" />
    <button v-bind:title="button" v-bind:on-press="handleBtnPress"/>
  </view>
</template>

<script>
export default {
  data: function() {
    return {
      message: "Hello World!",
      button: "Button"
    };
  },
  methods: {
    handleBtnPress: function() {
      alert('Btn Press');
    }
  }
};
</script>

<style>
  .text, .image {
    margin-bottom: 15px;
  }

  .container {
    flex: 1;
    background-color: white;
    align-items: center;
    justify-content: center;
  }
  .text-color-primary {
    color: blue;
    font-size: 30;
  }
</style>