<template>
  <div class="dashboard">
    <v-navigation-drawer dark permanent absolute>
      <v-list dense nav>
        <v-list-item class="mb-8 text-center">
          <v-list-item-content>
            <v-list-item-title
              class="text-h5 font-weight-bold secondary-text"
              >ANIMU</v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>

        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item @click="$router.push('/dashboard/collection')" link>
          <v-list-item-icon>
            <v-icon>mdi-collage</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Collection</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-container>
      <v-app-bar color="transparent" elevation="0">
        <v-btn
          color="primary"
          @click="connectWallet"
          v-if="!wallet.publicKey"
          ><v-icon left>mdi-wallet</v-icon> Connect Wallet</v-btn
        >
        <v-btn color="red" @click="disconnectWallet" v-else
          ><v-icon left>mdi-wallet</v-icon> Disconnect Wallet</v-btn
        >
        <v-spacer></v-spacer>

        <div class="d-flex align-center">
          <v-avatar class="mx-4">
            <v-img
              :src="`https://cdn.discordapp.com/avatars/${discordUser.id}/${discordUser.avatar}.webp?size=160`"
            ></v-img>
          </v-avatar>
          <div class="d-flex flex-column">
            <p class="ma-0">Collector</p>
            <v-menu left bottom offset-y rounded="lg">
              <template v-slot:activator="{ on, attrs }">
                <h5 v-bind="attrs" v-on="on">
                  Hey, {{ discordUser.username }}
                  <v-icon color="primary">mdi-chevron-down</v-icon>
                </h5>
              </template>

              <v-list class="glass-list" dense>
                <v-list-item
                  v-if="wallet.publicKey && !user.publicKey"
                  @click="verifyWallet"
                  link
                >
                  <v-list-item-icon
                    ><v-icon>mdi-wallet</v-icon></v-list-item-icon
                  >
                  <v-list-item-title>Verify Wallet</v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>
          </div>
        </div>
      </v-app-bar>

      <router-view></router-view>
    </v-container>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'DashboardLayout',
  mounted() {
    this.$store.dispatch('connectWalletEager');
  },
  computed: {
    discordUser() {
      return this.$store.state.discordUser;
    },
    user() {
      return this.$store.state.user;
    },
    wallet() {
      return this.$store.state.wallet;
    },
  },
  methods: {
    logout() {
      localStorage.removeItem('ANIMU_USER_TOKEN');
      this.$router.replace({
        name: 'Home',
      });
    },
    connectWallet() {
      this.$store.dispatch('connectWallet');
    },
    disconnectWallet() {
      this.$store.dispatch('disconnectWallet');
    },
    verifyWallet() {
      this.$store.dispatch('verifyWallet');
    },
  },
});
</script>

<style>
.dashboard {
  background-color: #130f40 !important;
  background-image: linear-gradient(
    135deg,
    #130f40 0%,
    #000000 74%
  ) !important;
}

.logout-btn {
  position: absolute;
  bottom: 40px;
  left: 50%;
  width: calc(100% - 16px);
  transform: translateX(-50%);
}
</style>
