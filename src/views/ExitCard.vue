<template>
    <ion-card>
        <ion-card-content>
          <span v-html="generateExit(ratio, ent, rk, ms)"></span>  
        </ion-card-content>
    </ion-card>
</template>
<script lang="ts">
import { IonCard, IonCardContent, } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ExitCard',
  props: ["ratio", "ent", "rk", "ms"],
  components: {
    IonCard, 
    IonCardContent,
  },
  methods: {
    generateExit(ratio: number, ent: any, rk: any, ms: any) {
      let entry : number = ent === '' ? 0 : Number(ent);
      if (rk <= 0 || entry <= 0 || ms <= 0) {
        return;
      }
      //for list that calls this, loop through settings.exits[] and grab the rest from data.
      // if manual input for exit then add to front of settings.exits[]
      let reward = Number((rk * ratio).toFixed(2));
      console.log(typeof entry);
      let exit = entry + reward;
      console.log(exit)
      let total_reward = reward * ms;
      console.log(total_reward)
      return '<ion-card-content class="flex_card_content"><span>1:'+ratio+'</span><span>'+exit.toFixed(2).toLocaleString()+'</span><span>'+reward.toFixed(2).toLocaleString()+'</span><span>'+total_reward.toFixed(2).toLocaleString()+'</span></ion-card-content>';
    },
  }
});
</script>
<style>
.flex_card_content {
  display: flex;
  justify-content: space-around;
  margin: 14px 0;
}
.flex_card_content span {
  flex: 1 0 1;
}
</style>