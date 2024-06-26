<template>
  <div class="app-navbar-mobile">
    <app-logo />
    <music-player />
    <button
      class="app-navbar-mobile__burger-btn"
      @click="isExtShown = !isExtShown"
    >
      <transition name="fade" mode="out-in">
        <app-icon
          class="app-navbar-mobile__burger-icon"
          :key="String(isExtShown)"
          :name="!isExtShown ? $icons.menuAlt1 : $icons.x"
        />
      </transition>
    </button>
    <transition name="fade" appear>
      <div v-if="isExtShown" class="app-navbar-mobile__ext-wrp">
        <transition name="slide" appear>
          <div class="app-navbar-mobile__ext">
            <transition name="fade" mode="out-in">
              <connect-wallet-button
                v-if="!web3ProvidersStore.isConnected"
                class="app-navbar-mobile__connect-wallet-btn"
                color="secondary"
              />
              <div v-else class="app-navbar-mobile__wallet-info-wrp">
                <network-switch />
                <wallet-balances />
                <wallet-dashboard />
              </div>
            </transition>
          </div>
        </transition>
        <button
          class="app-navbar-mobile__ext-backdrop"
          tabindex="-1"
          @click="closeExt"
        />
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import MusicPlayer from './MusicPlayer.vue'
import { useWeb3ProvidersStore } from '@/store'
import { ref } from 'vue'
import AppIcon from './AppIcon.vue'
import AppLogo from './AppLogo.vue'
import ConnectWalletButton from './ConnectWalletButton.vue'
// import NetworkSwitch from './NetworkSwitch.vue'
// import WalletBalances from './WalletBalances.vue'
import WalletDashboard from './WalletDashboard.vue'

const web3ProvidersStore = useWeb3ProvidersStore()

const isExtShown = ref(false)

const closeExt = () => {
  isExtShown.value = false
}
</script>

<style scoped lang="scss">
$background: #ffffff;
$z-index: 1000;

.app-navbar-mobile {
  position: fixed;
  z-index: $z-index;
  top: 0;
  height: var(--app-navbar-height);
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: toRem(24);
  padding: 0 2rem 0 2rem;
  background: $background;
  border-bottom: toRem(1) solid #CBD5E0;
}

.app-navbar-mobile__burger-icon {
  height: toRem(24);
  width: toRem(24);
  color: var(--text-secondary-dark);
  transition: var(--transition-duration-fast) var(--transition-timing-default);

  .app-navbar-mobile__burger-btn:not([disabled]):hover &,
  .app-navbar-mobile__burger-btn:not([disabled]):focus &,
  .app-navbar-mobile__burger-btn:not([disabled]):active & {
    color: var(--primary-main);
  }
}

.app-navbar-mobile__ext-wrp {
  position: fixed;
  overflow: hidden;
  inset: var(--app-navbar-height) 0 0;
  background: var(--backdrop-modal);
  width: 100%;
}

.app-navbar-mobile__ext {
  display: flex;
  flex-direction: column;
  background: $background;
  padding: toRem(28) var(--app-padding-right) toRem(32) var(--app-padding-left);
}

.app-navbar-mobile__nav {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: toRem(20);
}

.app-navbar-mobile .app-navbar-mobile__link {
  &.router-link-active {
    @include respond-to(medium) {
      font-family: var(--app-font-family);
      font-size: toRem(18);
      font-weight: 600;
      line-height: toRem(26);
      letter-spacing: 0;
    }
  }

  @include respond-to(medium) {
    font-family: var(--app-font-family);
    font-size: toRem(18);
    font-weight: 400;
    line-height: toRem(26);
    letter-spacing: 0;
  }
}

.app-navbar-mobile .app-navbar-mobile__connect-wallet-btn {
  margin: toRem(76) auto 0;
  min-width: toRem(202);
}

.app-navbar-mobile__wallet-info-wrp {
  display: flex;
  align-items: center;
  gap: toRem(20);
  margin: toRem(76) auto 0;

  @include respond-to(medium) {
    flex-direction: column;
  }
}

.app-navbar-mobile__ext-backdrop {
  $z-index: -1;

  position: absolute;
  cursor: default;
  z-index: $z-index;
  inset: 0;
  width: 100%;
}

.slide-enter-active {
  animation: slide-in var(--transition-duration-fast)
    var(--transition-timing-default);
}

.slide-leave-active {
  animation: slide-in var(--transition-duration-fast)
    var(--transition-timing-default) reverse;
}

@keyframes slide-in {
  from {
    transform: translateY(-100%);
  }

  to {
    transform: translateY(0);
  }
}
</style>
