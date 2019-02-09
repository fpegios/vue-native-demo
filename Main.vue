<template>
  <view class="container">
    <status-bar
      background-color="blue"
    />
    <scroll-view :content-container-style="{ paddingVertical: 20 }">
        <text>Main</text>
				<button class="button" title="Go to second" v-bind:on-press="goToSecond"/>
      <!-- <activity-indicator size="large" color="#0000ff" /> -->
      <!-- <camera class="camera" :type="this.type"/> -->
        <!-- <text class="text-color-primary">{{JSON.stringify(notification.data)}}</text> -->
      <text>Latitude: {{ location }}</text>
      <touchable-opacity :on-press="getLocation" >
          <text>get location</text>
      </touchable-opacity>
      <text>Accelerometer:</text>
      <text>{{accelerometerData.x}}</text>
      <text class="text text-color-primary">{{ message }}</text>
      <image class="image" :source="require('./assets/icon.png')"/>
      <button class="button" v-bind:title="button" v-bind:on-press="handleBtnPress"/>
      
      <text-input class="text-input" v-model="textInput"/>
      <button
        :on-press="onPressLearnMore"
        title="Learn More"
        color="#841584"
        accessibility-label="Learn more about this purple button"
      />
      <flat-list
          :data="[{key: 'a'}, {key: 'b'}]"
          :render-item="(item) => renderList(item)"
      />
      <switch
        :on-value-change = "handleChange"
        :value = "value"/>
      <touchable-opacity :on-press="onPressButton">
        <image :source="require('./assets/icon.png')" />
      </touchable-opacity>
      <web-view
        :source="{uri: 'https://github.com/facebook/react-native'}"
        :style="{marginTop: 20}"
      />
    </scroll-view>
  </view>
</template>

<script>
import React from 'react';
import { Text, Alert } from 'react-native';
import { Camera, Accelerometer, Constants, Location, Permissions, Notifications } from "expo";

export default {
	props: {
		navigation: {
			type: Object
		}
	},
  components: { 
    Camera 
  },
  data: function() {
    return {
      message: "Hello World!",
      button: "Button",
      textInput: "",
      value: false,
      accelerometerData: {},
      location: {},
      errorMessage: "",
      hasCameraPermission: false,
      type: Camera.Constants.Type.back,
      notification: {}
    };
  },
  methods: {
    goToSecond() {
        this.navigation.navigate("Second");
    },
    handleBtnPress: function() {
      alert('Btn Press');
    },
    onPressLearnMore: function() {
        Alert.alert(
            'Alert Title',
            'My Alert Msg',
            [
                {text: 'Ask me later', onPress: () => console.log('Ask me later pressed')},
                {text: 'Cancel', onPress: () => console.log('Cancel Pressed'), style: 'cancel'},
                {text: 'OK', onPress: () => console.log('OK Pressed')},
            ],
            { cancelable: false }
        );
    },
    renderList: function(item) {
      return (<Text>{item.item.key}</Text>)
    },
    handleChange: function(val) {
      this.value = val;
    },
    onPressButton: function() {
      alert('Clicked Image')
    },
    getLocation: function() {
      Permissions.askAsync(Permissions.LOCATION).then(status => {
        if (status !== "granted") {
          this.errorMessage = "Permission to access location was denied";
          console.log(this.errorMessage);
        }
        Location.getCurrentPositionAsync({}).then(location1 => {
          this.location = location1.coords.latitude;
        });
      }).catch((err)=>{
        console.log(err);
     });
    },
    _handleNotification: function(notification) {
      this.notification = notification;
    },
    registerForPushNotificationsAsync: async function() {
      const { status: existingStatus } = await Permissions.getAsync(
        Permissions.NOTIFICATIONS
      );
      let finalStatus = existingStatus;

      // only ask if permissions have not already been determined, because
      // iOS won't necessarily prompt the user a second time.
      if (existingStatus !== "granted") {
        // Android remote notification permissions are granted during the app
        // install, so this will only ask on iOS
        const { status } = await Permissions.askAsync(
          Permissions.NOTIFICATIONS
        );
        finalStatus = status;
      }

      // Stop here if the user did not grant permissions
      if (finalStatus !== "granted") {
        return;
      }

      // Get the token that uniquely identifies this device
      Notifications.getExpoPushTokenAsync().then(token => {
        console.log(token);
      });
    }
  },
  created() {
    this.registerForPushNotificationsAsync();
    this._notificationSubscription = Notifications.addListener(
      this._handleNotification
    );
  },
  mounted() {
    // Accelerometer.addListener(accelerometerData => {
    //   this.accelerometerData = accelerometerData;
    // });

    // Accelerometer.setUpdateInterval(1000);

    // Permissions.askAsync(Permissions.CAMERA)
    //  .then(status => {
    //    this.hasCameraPermission = status.status == "granted" ? true : false;
    //  }).catch((err)=>{
    //     console.log(err);
    //  });
  }
};
</script>

<style>
  .text, .image, .button {
    margin-bottom: 15px;
  }

  .camera {
    width: 100%;
    height: 500px;
  }

  .container {
    background-color: white;
  }

  .text-color-primary {
    color: blue;
    font-size: 30;
  }

  .text-input {
    height: 60px;
    font-size: 30px;
    width: 50%;
  }
</style>