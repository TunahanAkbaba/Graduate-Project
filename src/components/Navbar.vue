<template>
    <nav>

        <v-snackbar color="primary" top v-model="snackbar" :timeout="4000">
            <span>Başarılı! Yeni Proje Eklendi.</span>
            <template v-slot:action="{ attrs }">
            <v-btn color="success" text v-bind="attrs" @click="snackbar = false">KAPAT</v-btn>
            </template>
        </v-snackbar>


        <v-app-bar flat app>
            <v-app-bar-nav-icon class="grey--text" @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-toolbar-title class="text-uppercase grey--text">
                <span class="font-weight-light">Proje</span>
                <span>YÖNETİM</span>
            </v-toolbar-title>
            <v-spacer></v-spacer>
            
            <v-btn @click="logout" text color="grey" :loading="loading">
                <span>Çıkış Yap</span>
                <v-icon right>mdi-exit-to-app</v-icon>
            </v-btn>
        </v-app-bar>
    <v-navigation-drawer app v-model="drawer" class="primary" >
       <v-row justify="center">
        <v-col cols="6">
            <v-avatar v-if="currentUser==='g151210020@sakarya.edu.tr'" size="100">
            <img src="/avatar-1.jpg" />
          </v-avatar>
          <v-avatar v-if="currentUser==='b151210020@sakarya.edu.tr'" size="100">
            <img src="/avatar-2.jpg" />
          </v-avatar>
          
          
          
        </v-col>
      </v-row>
      <v-btn text block class="large primary" v-if="currentUser==='g151210020@sakarya.edu.tr'">
                İBRAHİM GÜN
            </v-btn>
                  <v-btn text block class="large primary" v-if="currentUser==='b151210020@sakarya.edu.tr'">
                HİLMİ TUNAHAN AKBABA
            </v-btn>
           
      
      <v-row class="mt-4 mb-3" justify="center">
          <Popup @projectAdded="snackbar = true" />
      </v-row>
        <v-list>
         <v-list-item v-for="link in links" :key="link.text" router :to="link.route"> 
             <v-list-item-action>
                 <v-icon class="white--text">{{ link.icon }}</v-icon>
             </v-list-item-action>
             <v-list-item-content>
                 <v-list-item-title class="white--text"> {{ link.text }}</v-list-item-title>
             </v-list-item-content>
         </v-list-item>
        </v-list>
    </v-navigation-drawer>


    </nav>
</template>


<script>
import Popup from './Popup'
import firebase from 'firebase'

export default {
    components: { Popup },
    data() {
        return {
        drawer: true,
        links: [
            { icon: 'mdi-view-dashboard', text: 'Kontrol Paneli', route: '/' },
            { icon: 'mdi-folder', text: 'Projelerim', route: '/projects' },
            { icon: 'mdi-account-group', text: 'Takım', route: '/team' }
        ],
        snackbar: false,
        isLoggendIn: false,
        currentUser: false,
        loading: false
        }
    },
    created() {
        if (firebase.auth().currentUser) {
            this.isLoggendIn = true
            this.currentUser = firebase.auth().currentUser.email
        }

    },
    methods: {
        logout: function() {
            firebase.auth().signOut().then(() => {
                this.$router.push('/login')
            })
        }
    }
}
</script>