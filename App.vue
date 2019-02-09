<template>
  <app-navigation></app-navigation>
</template>

<script>
import { StackNavigator } from "vue-native-router";
import Main from "./Main.vue";
import Second from "./Second.vue";

const AppNavigation = StackNavigator(
  {
    Main: {
      screen: Main,
      navigationOptions: () => ({
        title: 'Main',
        headerBackTitle: 'BACK',
        headerTruncatedBackTitle: 'to Second'
      })
    },
    Second: {
      screen: Second,
      navigationOptions: () => ({
        title: 'Second',
        headerBackTitle: 'BACK',
        headerTruncatedBackTitle: 'to Main'
      })
    }
  },
  {
    initialRouteName: 'Main'
  }
);
export default {
    components: { AppNavigation }
}
</script>