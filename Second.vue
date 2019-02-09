<template>
  <view class="container">
    <status-bar
      background-color="blue"
    />
    <text>Secondary</text>
		<button class="button" title="Go to main" v-bind:on-press="goToMain"/>
  </view>
</template>

<script>

export default {
	props: {
		navigation: {
			type: Object
		}
	},
  data: function() {
    return {
      
    };
	},
	methods: {
    goToMain() {
			this.navigation.navigate("Main");
		}
	}
};
</script>

<style>

</style>