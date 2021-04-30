<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Register</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <div id="container">
        <strong>Ready to create an app?</strong>
        <p>Start with Ionic <a target="_blank" rel="noopener noreferrer" href="https://ionicframework.com/docs/components">UI Components</a></p>

        <form @submit.prevent="onFormSubmit">
          <ion-grid>
            <ion-row>
              <ion-col>
                <ion-item>
                  <ion-label position="stacked">Name</ion-label>
                  <ion-input type="text" v-model="user.name" required></ion-input>
                </ion-item>
              </ion-col>
            </ion-row>

            <ion-row>
              <ion-col>
                <ion-item>
                  <ion-label position="stacked">Email</ion-label>
                  <ion-input type="email" v-model="user.email" required></ion-input>
                </ion-item>
              </ion-col>
            </ion-row>

            <ion-row>
              <ion-col>
                <ion-item>
                  <ion-label position="stacked" type="email">Password</ion-label>
                  <ion-input type="password" v-model="user.password" required></ion-input>
                </ion-item>
              </ion-col>
            </ion-row>

            <ion-row>
              <ion-col>
                <ion-button expand="block" @click="() => router.push('/Home')" color="danger">Back To Home Page</ion-button>
              </ion-col>
              <ion-col>
                <ion-button expand="block"  type="Submit">Submit</ion-button>
              </ion-col>
            </ion-row>
          </ion-grid>
        </form>

      </div>
    </ion-content>
  </ion-page>
</template>


<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonLabel, IonInput, IonItem, IonButton, IonCol, IonGrid, IonRow  } from '@ionic/vue';
import { defineComponent } from 'vue';

import { useRouter } from 'vue-router';

import axios from 'axios';

import { Drivers, Storage } from '@ionic/storage';
import * as CordovaSQLiteDriver from 'localforage-cordovasqlitedriver';
const storage = new Storage({
  driverOrder: [CordovaSQLiteDriver._driver, Drivers.IndexedDB, Drivers.LocalStorage]
});

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonLabel, IonInput, IonItem,
    IonButton,
    IonCol, IonGrid, IonRow
  },
  setup() {
    const router = useRouter();
    return { router };
  },
 data() {
      return {
          user: {
             name: '',
             email: '',
             password: ''
          }
      }
  },
  methods: {
      onFormSubmit() { 
        // await storage.remove(key);
        //const storage = new Storage();
        storage.create();

        //storage.clear();

        alert(JSON.stringify(this.user)) 
        axios.post('http://188.166.187.2:8082/api/register/create', this.user)
          .then(res => {

          if(res.data.response=='Failed'){
            alert('Existing Email');
          } else if(res.data.response=='Error'){
            alert('Error');
          } else {

            this.$router.push({
              name: 'Login',
              params: { id:'123' }
            });

          }
          //console.log(res.data);
        })
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
  top: 50%;
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
</style>