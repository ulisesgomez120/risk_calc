<template>
  <ion-page> 

    <ion-content :fullscreen="true">  
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
        <div class="instructions" v-if="risk <= 0">
          <ion-item>
            <ion-label class="ion-text-wrap">Enter an entry and stop loss to generate exits</ion-label>
          </ion-item>
          <ion-item>
            <ion-label class="ion-text-wrap">Enter an exit price to calculate a reward:risk ratio</ion-label>
          </ion-item>
        </div>
        <div class="generate_exits" v-else>
            <ion-item>
              <ion-label class="calculated_data">
                Risk Per Share
              </ion-label>
              <ion-label class="calculated_data">
                Cost To Enter
              </ion-label>
              <ion-label class="calculated_data">
                Shares
              </ion-label>
            </ion-item>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonPage, IonLabel, IonInput, IonItem, IonSegment, IonSegmentButton, } from '@ionic/vue';
import { defineComponent } from 'vue';

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
  },
  data() {
    return {
      entry: '',
      stop_loss: '',
      manual_exit: '',
      direction: 'long',
      risk: 0
    }
  },
  // watch: {
  //   // entry(new_val) {
  //   //   let risk = 0;
  //   //   let stop_loss = this.stop_loss === '' ? 0 : this.stop_loss;
  //   //   console.log(new_val);
  //   //   if (new_val > 0 && stop_loss > 0) {
  //   //     if (this.direction === 'long') {
  //   //       risk = +(new_val) - +(stop_loss)
  //   //     } else {
  //   //       risk = +(stop_loss) - +(new_val)
  //   //     }
  //   //   }
  //   //   this.risk = risk;
  //   //   console.log(this.risk)
  //   // },
  //   // stop_loss(new_val) {
  //   //   let risk = 0;
  //   //   let entry = this.entry === '' ? 0 : this.entry;
  //   //   console.log(new_val);
  //   //   if (entry > 0 && new_val > 0) {
  //   //     if (this.direction === 'long') {
  //   //       risk = +(entry) - +(new_val)
  //   //     } else {
  //   //       risk = +(new_val) - +(entry)
  //   //     }
  //   //   }
  //   //   this.risk = risk;
  //   // }
  // },
  methods: {
    segmentChanged (e:any) {
      // use for icon change, rotate on change
      console.log(e.detail.value);
    },
    calc_risk () {
      let risk = 0;
      let stop_loss = this.stop_loss === '' ? 0 : this.stop_loss;
      let entry = this.entry === '' ? 0 : this.entry;
      console.log(stop_loss, entry);
      if (entry > 0 && stop_loss > 0) {
        if (this.direction === 'long') {
          risk = +(entry) - +(stop_loss)
        } else {
          risk = +(stop_loss) - +(entry)
        }
      }
      this.risk = risk;
    }

  }
});

</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 65%;
  transform: translateY(-50%);
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
.calculated_data {
  text-align: center;
}
.price_inputs {
  margin-bottom: 12px;
}

.segment {
  margin-bottom: 20px;
}
</style>
