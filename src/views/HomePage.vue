<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>{{ $route.params.id }}</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-refresher slot="fixed" @ionRefresh="refresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Inbox</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-list>
        <MessageListItem v-for="message in messages" :key="message.id" :message="message" />
      </ion-list>

      <ion-fab slot="fixed" vertical="bottom" horizontal="end">
        <ion-fab-button>
          <ion-icon id="open-modal" :icon="add"></ion-icon>
        </ion-fab-button>
        <ion-modal ref="modal" trigger="open-modal" @willDismiss="onWillDismiss">
          <ion-header>
            <ion-toolbar>
              <ion-buttons slot="start">
                <ion-button @click="cancel()">Cancel</ion-button>
              </ion-buttons>
              <ion-title>Welcome</ion-title>
              <ion-buttons slot="end">
                <ion-button :strong="true" @click="confirm()">Confirm</ion-button>
              </ion-buttons>
            </ion-toolbar>
          </ion-header>
          <ion-content class="ion-padding">
            <ion-item>
              <ion-input label="Your Name" label-placement="floating" placeholder="Enter text"></ion-input>
            </ion-item>
          </ion-content>
        </ion-modal>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonList,
  IonPage,
  IonRefresher,
  IonRefresherContent,
  IonTitle,
  IonToolbar,
  IonButtons,
  IonMenuButton,
  IonFab,
  IonFabButton,
  IonFabList,
  IonIcon,
  IonModal,
  IonButton,
  IonItem,
  IonLabel,
  IonInput
} from '@ionic/vue';
import MessageListItem from '@/components/MessageListItem.vue';
import { getMessages, Message } from '@/data/messages';
import { OverlayEventDetail } from '@ionic/core/components';
import { add } from 'ionicons/icons';


import { defineComponent, ref } from 'vue';

export default defineComponent({
  components: {
    MessageListItem,
    IonContent,
    IonHeader,
    IonList,
    IonPage,
    IonRefresher,
    IonRefresherContent,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonMenuButton,
    IonFab,
    IonFabButton,
    IonFabList,
    IonIcon,
    IonModal,
    IonButton,
    IonItem,
    IonLabel,
    IonInput,
  },
  setup() {
    const messages = ref<Message[]>(getMessages());


    const refresh = (ev: CustomEvent) => {
      setTimeout(() => {
        ev.detail.complete();
      }, 3000);
    }

    return { messages, refresh, add }
  },
  data() {
    return {
      message: 'This modal example uses triggers to automatically open a modal when the button is clicked.',
    };
  },
  methods: {
    cancel() {
      this.$refs.modal.$el.dismiss(null, 'cancel');
    },
    confirm() {
      const name = this.$refs.input.$el.value;
      this.$refs.modal.$el.dismiss(name, 'confirm');
    },
    onWillDismiss(ev: CustomEvent<OverlayEventDetail>) {
      if (ev.detail.role === 'confirm') {
        this.message = `Hello, ${ev.detail.data}!`;
      }
    },
  },
})
</script>
