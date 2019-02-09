<template>
  <view class="container">
    <text class="text text-color-primary">{{ message }}</text>
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
  .text {
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