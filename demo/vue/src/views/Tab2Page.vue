<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 2</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 2</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <ExploreContainer name="Tab 2 page" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import { EmbeddedWebView, EmbeddedWebviewOptions, EmbeddedWebviewConfiguration } from '@skomiyama/embedded-webview';

import ExploreContainer from '@/components/ExploreContainer.vue';


export let initializedWebView = false;

export default defineComponent({
  name: 'Tab2Page',
  // component: {
  //   initializedWebView: Boolean
  // },
  components: { ExploreContainer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  async mounted() {
    const configuration: EmbeddedWebviewConfiguration = {
      styles: {
        width: (this.$el as HTMLElement).clientWidth,
        height: ((this.$el as HTMLElement).clientHeight),
      },
      global: {
        parent: {
          pet: 'dog',
          children: ['boy1', 'girl1']   
        }
      }
    }
    const options: EmbeddedWebviewOptions = {
      url: 'http://localhost:3000',
      path: '/second',
      configuration
    }
    await EmbeddedWebView.create(options);
  },
  async ionViewDidEnter() {
    if (initializedWebView) {
      console.log(initializedWebView)
      console.log(await EmbeddedWebView.show());
    }
  },
  async ionViewDidLeave() {
    initializedWebView = true
    console.log(await EmbeddedWebView.hide());
  }
});
</script>
