<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <h2><span>Settings</span></h2>
      <div class="setting_section">
        <div class="form_group_flex">
          <ion-label>Account Balance:</ion-label>
          <ion-input type="number" :model="account_balance" placeholder="Account Balance"></ion-input>
        </div>
      </div>
      <!-- risk section -->
      <h3>Risk</h3>
      <div class="setting_section">
        <div class="form_group_flex">
          <ion-label>Percentage of Account:</ion-label>
          <ion-input type="number" min="0" max="100" :model="account_balance" placeholder="Percentage"></ion-input>
        </div>
        <div class="form_group_flex">
          <ion-label>Max Dollar Amount:</ion-label>
          <ion-input type="number" min="0" max="100" :model="account_balance" placeholder="$$ Amount"></ion-input>
        </div>
        <ion-segment class="segment" @ionChange="active_risk($event)" value="dollar">
          <ion-segment-button value="percentage">
            <ion-label>%</ion-label>
          </ion-segment-button>
          <ion-segment-button value="dollar">
            <ion-label>$$</ion-label>
          </ion-segment-button>
        </ion-segment>
      </div>
      <!-- exit section -->
      <div class="setting_exit_heading_container">
        <h3>Exits</h3>
        <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
      </div>
      <div class="setting_exit_cards_container">
        <div class="setting_exit_cards">
          <div class="setting_exit_card_action_container">
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
          </div>
          <h3>3x</h3>
        </div>
        <div class="setting_exit_cards">
          <div class="setting_exit_card_action_container">
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
          </div>
          <h3>3x</h3>
        </div>
        <div class="setting_exit_cards">
          <div class="setting_exit_card_action_container">
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
          </div>
          <h3>3x</h3>
        </div>
        <div class="setting_exit_cards">
          <div class="setting_exit_card_action_container">
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
            <ion-button @click="add_exit" id="settings_btn" size="small" fill="clear"><ion-icon slot="icon-only" :src="add_icon"></ion-icon></ion-button>
          </div>
          <h3>3x</h3>
        </div>
      </div>
      <!-- close settings button -->
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button size="small" @click="close_settings">
          <ion-icon :src="close_icon"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
/* eslint-disable */

import { IonContent, IonPage, IonLabel, IonInput, IonItem, IonSegment, IonSegmentButton, IonIcon, IonButton, IonCard, IonCardHeader, IonCardContent, IonFab, IonFabButton,} from '@ionic/vue';
import { defineComponent } from 'vue';
import { closeOutline, addCircleOutline} from 'ionicons/icons';
/* eslint-disable */

export default defineComponent({
  name: 'SettingsPage',
  components: {
    IonContent,
    IonPage,
    IonInput,
    IonItem,
    IonLabel,
    IonSegment,
    IonSegmentButton,
    IonIcon,
    IonButton,
    IonCard,
    IonCardContent,
    IonFab, 
    IonFabButton,
    IonCardHeader,
},
  data() {
    return {
        close_icon: closeOutline,
        add_icon: addCircleOutline,
        account_balance: 0
    }
  },watch: {

  },
  methods: {
    close_settings() {
      this.$router.back();
    },
    active_risk(e:any) {
      console.log(e.detail.value);
    },
    add_exit() {
      console.log('add exit')
    }
  }
});
</script>

<style scoped>
ion-fab-button {
  --background: #b0eed9;
  --background-activated: #89c7b3;
  --color: #333;
}
ion-label {
  flex: 0 0 45%;
}
ion-input {
  flex: 0 0 45%;
}
h2 {
  text-align: center;
  margin: 16px 0 8px;
}
h2 span {
  border-bottom: solid 1px #30487b;
  padding: 0 16px 8px;
}
h3 {
  margin-left: 17px;
}
.setting_section {
  margin: 18px 17px;
  padding: 8px 0 14px;
  border-bottom: solid 1px #30487b;
}
.form_group_flex {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}
.segment {
  margin: 20px 0;
  padding: 0 8px;
}
.setting_exit_heading_container {
  display: flex;
  justify-content: space-between;
  margin: 0 17px;
}
.setting_exit_heading_container h3{
  margin: 0;  
}
.setting_exit_cards_container {
  margin: 8px 24px 120px;
  border-bottom: solid 1px #30487b;
  border-top: solid 1px #30487b;
  overflow-x: scroll;
  display: flex;
}
.setting_exit_cards {
  width: 117px;
  height: 145px;
  margin: 12px;
  background: orange;
  border-radius: 13px;
}
.setting_exit_card_action_container {
  display: flex;
  padding: 13px 0;
}
.setting_exit_card_action_container ion-button {
  flex: 0 0 50%;
  margin: 0;
}
.setting_exit_cards h3 {
  margin: 0;
  padding: 12px 0;
  text-align: center;
  background-color: rgba(0,0,0,0.25);
}
</style>
<!-- #081227 bg color main  -->