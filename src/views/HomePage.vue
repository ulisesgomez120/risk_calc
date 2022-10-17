<template>
  <ion-page> 
    <ion-content :fullscreen="true">
      <div id="logo_container">
        <ion-img src="assets/icon/Logo.png"></ion-img> 
      </div>
      <ion-button @click="show_settings" id="settings_btn" size="small" fill="clear"><ion-icon slot="start" :src="settings_icon"></ion-icon>Settings</ion-button>
      <div id="container">
        <!-- form -->
        <ion-item class="price_inputs">
          <ion-label position="floating">Entry Price:</ion-label>
          <ion-input type="number" @change="calc_risk" v-model.number="entry"></ion-input>
        </ion-item>
        <ion-item class="price_inputs">
          <ion-label position="floating">Stop Loss:</ion-label>
          <ion-input type="number" @change="calc_risk" v-model.number="stop_loss"></ion-input>
        </ion-item>
        <ion-item class="price_inputs">
          <ion-label position="floating">Exit Price:</ion-label>
          <ion-input type="number" @change="calc_risk" v-model.number="manual_exit"></ion-input>
        </ion-item>
        <!-- segment -->
        <ion-segment class="segment" @ionChange="segmentChanged($event)" value="long">
          <ion-segment-button value="long">
            <ion-label>Long</ion-label>
          </ion-segment-button>
          <ion-segment-button value="short">
            <ion-label>Short</ion-label>
          </ion-segment-button>
        </ion-segment>
        <!-- just to show risk for now -->
        <div class="instructions" v-if="risk < 0">
          <ion-item>
            <ion-label class="ion-text-wrap">Enter an entry and stop loss to generate exits</ion-label>
          </ion-item>
          <ion-item>
            <ion-label class="ion-text-wrap">Enter an exit price to calculate a reward:risk ratio</ion-label>
          </ion-item>
        </div>
        <div v-else>
          <div class="generate_exits">
            <div class="calculated_data">
              <p>Risk Per Share</p>
              <p>{{risk}}</p>
            </div>
            <div class="calculated_data">
              <p>Cost To Enter</p>
              <p>{{cost_to_enter.toLocaleString()}}</p>
            </div>
            <div class="calculated_data">
              <p>Shares</p>
              <p>{{max_shares}}</p>
            </div>
          </div>
          <ion-card >
            <ion-card-content class="flex_card_content flex_card_content__header"><span>Risk:Reward</span><span>Exit</span><span>RPS</span><span>Total Reward</span></ion-card-content>
          </ion-card>
          <exit-card v-for="num in exit_list" :key="num" :ratio="num" :ent="entry" :rk="risk" :ms="max_shares">
          </exit-card>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonPage, IonLabel, IonInput, IonItem, IonSegment, IonSegmentButton, IonImg, IonIcon, IonButton, IonCard, IonCardContent, } from '@ionic/vue';
import ExitCard from './ExitCard.vue';
import { defineComponent } from 'vue';
import { settingsSharp } from 'ionicons/icons';
/* eslint-disable */

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonPage,
    IonInput,
    IonItem,
    IonLabel,
    IonSegment,
    IonSegmentButton,
    IonImg,
    IonIcon,
    IonButton,
    IonCard,
    IonCardContent,
    ExitCard,
},
  data() {
    return {
      entry: '',
      stop_loss: '',
      manual_exit: '',
      direction: 'long',
      max_shares: 0,
      cost_to_enter: 0,
      exit_list: [3,5,7],
      settings_icon: settingsSharp,
      settings: {
        account_balance: 1400,
        max_risk: {
          percentage: .05,
          dollar_amount: 0
        },
        max_position_dollar_amount: 300,
        calc_cost_to_enter_using: 'mp', //mp = max_position, mr = max_risk
      },
      //also effect visuals
      risk: 0,
      show_exits: false,
    }
  },watch: {
    risk(new_val) {
      if (new_val > 0) {
        // generate shares, cost to enter function 
        let entry = this.entry == '' ? 0 : Number(this.entry);
        if (this.settings.calc_cost_to_enter_using === 'mr') {
          if (this.settings.max_risk.dollar_amount > 0) {
            this.max_shares = Math.floor(this.settings.max_risk.dollar_amount / new_val);
            let entry = this.entry == '' ? 0 : Number(this.entry);
            this.cost_to_enter = this.max_shares * entry;
          } else if (this.settings.max_risk.percentage > 0) {
            let dollars = Math.floor(this.settings.account_balance * this.settings.max_risk.percentage);
            console.log(dollars);
            this.max_shares = Math.floor(dollars / new_val);
            let entry = this.entry == '' ? 0 : Number(this.entry);
            this.cost_to_enter = this.max_shares * entry;
          }
        } else {
          console.log(this.settings.max_position_dollar_amount, entry)
          this.max_shares = Math.floor(this.settings.max_position_dollar_amount / entry);
          this.cost_to_enter = this.max_shares * entry;
        }
        
      }
    }
  },
  methods: {
    segmentChanged (e:any) {
      // use for icon change, rotate on change
      console.log(e.detail.value);
    },
    calc_risk () {
      let risk = 0;
      let stop_loss = this.stop_loss === '' ? 0 : this.stop_loss;
      let entry = this.entry === '' ? 0 : this.entry;
      if (entry > 0 && stop_loss > 0) {
        if (this.direction === 'long') {
          risk = Number((Number(entry) - Number(stop_loss)).toFixed(2));
        } else {
          risk = Number((Number(stop_loss) - Number(entry)).toFixed(2));
        }
      }
      this.risk = risk;
    },
    show_settings() {
      console.log('settings');
      this.$router.push({name: 'Settings'})
    }

  }
});

</script>

<style scoped>
#logo_container {
  height: 55%;
  display: flex;
  justify-content: center;
  align-items: center;
}
#logo_container ion-img::part(image) {
  height: 180px;
  width: 180px;
}
#settings_btn {
  position: absolute;
  right: 0;
  top: 45px;
  color: lightgray;
}
#container {
  text-align: center;
  /* position: absolute;
  left: 0;
  right: 0;
  top: 65%; */
  /* transform: translateY(-50%); */
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
.generate_exits {
  display: flex;
  justify-content: space-around;
}
.calculated_data {
  text-align: center;
  width: 30%;
  display: flex;
  flex-direction: column;
}
.calculated_data:nth-of-type(2) {
  border-left: 1px solid gray;
  border-right: 1px solid gray;
}
.price_inputs {
  margin-bottom: 12px;
}
.flex_card_content {
  display: flex;
  justify-content: space-between;
  margin: 14px 0;
  padding-left: 10px;
  padding-right: 10px;
}
.flex_card_content__header {
  font-size: bold;
}
.segment {
  margin: 20px 0;
  padding: 0 8px;
}
/* .rotate {
  transform: rotate('180deg');
} */
</style>
