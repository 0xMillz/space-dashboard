<template>
  <div class="app-navbar">
    <app-logo />
    <transition name="fade" mode="out-in">
      <connect-wallet-button
        v-if="!web3ProvidersStore.isConnected"
        class="app-navbar__connect-wallet-btn"
        color="secondary"
      />
      <div v-else class="app-navbar__wallet-info-wrp">
        <network-switch />
        <wallet-balances />
        <wallet-dashboard />
      </div>
    </transition>
  </div>
</template>

<script lang="ts" setup>
import { useWeb3ProvidersStore } from '@/store'
// import AppButton from './AppButton.vue'
import AppLogo from './AppLogo.vue'
import ConnectWalletButton from './ConnectWalletButton.vue'
// import NetworkSwitch from './NetworkSwitch.vue'
// import WalletBalances from './WalletBalances.vue'
import WalletDashboard from './WalletDashboard.vue'

const web3ProvidersStore = useWeb3ProvidersStore()
</script>

<style lang="scss" scoped>
$z-index: 1000;

.app-navbar {
  position: fixed;
  z-index: $z-index;
  top: 0;
  height: var(--app-navbar-height);
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: toRem(24);
  padding: 0 var(--app-padding-right) 0 var(--app-padding-left);
  background: #ffffff;
  border-bottom: none;
}

.app-navbar__nav {
  display: flex;
  gap: toRem(48);
}

.app-navbar .app-navbar__link {
  @include body-3-regular;

  &.router-link-active {
    @include body-3-semi-bold;
  }
}

.app-navbar .app-navbar__connect-wallet-btn {
  justify-self: right;
}

.app-navbar__wallet-info-wrp {
  display: flex;
  align-items: center;
  gap: toRem(20);
}
</style>
