<template>
  <app-navigator></app-navigator>
</template>

<script>
import {
  createAppContainer,
  createBottomTabNavigator,
  // createMaterialTopTabNavigator,
  createStackNavigator,
} from "vue-native-router";

// import HomeScreen from "./screens/HomeScreen.vue";
import GameScreen from "./screens/GameScreen.vue";
import SettingsScreen from "./screens/SettingsScreen.vue";
const IOSTabs = createBottomTabNavigator({
  Game: GameScreen,
  Settings: SettingsScreen,
});
// const AndroidTabs = createMaterialTopTabNavigator({
// Details: DetailsScreen,
// Settings: SettingsScreen,
// });
const StackNavigator = createStackNavigator({
  IOSTabs,
  // AndroidTabs,
  // Home: HomeScreen,
});
const AppNavigator = createAppContainer(StackNavigator);
export default {
  components: { AppNavigator },
};
</script>