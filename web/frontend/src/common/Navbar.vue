<template>
  <div>
    <b-navbar
      v-if="!inMobileWebView"
      toggleable="xl"
      :class="{
        'navbar-dark': theme === themes.Dark,
        'navbar-light': theme === themes.Light
      }">
      <b-container class="p-0">
        <b-navbar-brand href="/">
          <svg viewBox="0 0 1965 240" width="232" height="28.34">
            <use href="#svg-navbar-brand" />
          </svg>
        </b-navbar-brand>

        <b-navbar-toggle target="navbar-toggle-collapse">
          <template>
            <span class="navbar-toggler-icon"></span>
          </template>
        </b-navbar-toggle>

        <b-collapse id="navbar-toggle-collapse" is-nav v-model="showMainMenu">
          <b-navbar-nav>
            <b-nav-item v-if="user" href="/printers/" :class="{'active': viewName.includes('printers')}">Printer</b-nav-item>
            <b-nav-item v-if="user" href="/prints/" :class="{'active': viewName.includes('prints')}">Time-lapse</b-nav-item>
            <b-nav-item v-if="user" href="/gcodes/" :class="{'active': viewName.includes('gcodes')}">G-Code</b-nav-item>
            <b-nav-item v-if="!user" href="/publictimelapses/" :class="{'active': viewName === 'publictimelapse_list'}" class="glowing">Spaghetti Gallery</b-nav-item>
            <b-nav-item v-if="isEnt" href="/ent/pricing/" :class="{'active': viewName === 'pricing'}">Pricing</b-nav-item>
            <b-nav-item href="https://www.thespaghettidetective.com/help/">Help</b-nav-item>
            <b-nav-item href="https://discord.gg/hsMwGpD">Forum</b-nav-item>
          </b-navbar-nav>

          <b-navbar-nav class="ml-auto">
            <b-nav-item v-if="!user" href="/accounts/login/">Sign In</b-nav-item>
            <b-nav-item v-if="!user && allowSignUp" href="/accounts/signup/">Sign Up</b-nav-item>
            <b-nav-item v-if="isEnt && user" href="/ent/subscription/#detective-hour-balance" link-classes="badge-btn">
              <svg viewBox="0 0 384 550" width="14.66" height="21">
                <use href="#svg-detective-hours" />
              </svg>
              <span id="user-credits" class="badge badge-light">{{dhBadgeNum}}</span>
              <span class="sr-only">Detective Hours</span>
            </b-nav-item>
            <b-nav-item-dropdown v-if="user" ref="accountDropdown" right toggle-class="user-menu" :text="user.first_name || user.email">
              <b-dropdown-item href="/user_preferences/">
                <i class="fas fa-sliders-h mr-2"></i>Preferences
              </b-dropdown-item>
              <b-dropdown-item v-if="isEnt" href="/ent/subscription/">
                <i class="far fa-user-circle mr-2"></i>Account
              </b-dropdown-item>
              <b-dropdown-divider></b-dropdown-divider>
              <b-dropdown-item href="/accounts/logout/">
                <i class="fas fa-sign-out-alt mr-2"></i>Log out
              </b-dropdown-item>
            </b-nav-item-dropdown>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
import { inMobileWebView, user, settings } from '@lib/page_context'
import { Themes, theme } from '../main/themes.js'

export default {
  name: 'Navbar',

  components: {},

  data() {
    return {
      user: null,
      allowSignUp: false,
      isEnt: false,
      themes: Themes,
      showMainMenu: false,
    }
  },

  props: {
    viewName: {
      default() {return ''},
      type: String,
    },
  },

  created() {
    const {ACCOUNT_ALLOW_SIGN_UP, IS_ENT} = settings()
    this.allowSignUp = !!ACCOUNT_ALLOW_SIGN_UP
    this.isEnt = !!IS_ENT
    this.user = user()
  },

  computed: {
    inMobileWebView() {
      return inMobileWebView()
    },

    theme() {
      return theme.value
    },

    dhBadgeNum() {
      if (this.user && this.user.is_dh_unlimited) {
        return'\u221E'
      } else {
        return Math.round(this.user.dh_balance)
      }
    },
  },

  methods: {
    hideDropdowns() {
      this.showMainMenu = false

      // Check account dropdown (preferences and logout)
      const accountDropdown = this.$refs.accountDropdown
      if (accountDropdown) {
        accountDropdown.hide()
      }
    }
  },
}
</script>

<style lang="sass" scoped>
::v-deep .navbar
  padding: 0.5rem 1rem

  a.navbar-brand
    margin-top: -3px

    img
      width: 232px

  .nav-item
    text-transform: uppercase

  .user-menu
    text-transform: none

  .badge-btn
    position: relative
    height: 1.8rem
    margin-right: 1.5em

    img
      height: 1.3rem

    .badge
      position: absolute
      left: 22px
      top: 1px
      height: 18px
      border-radius: 4px
      transition: transform 0.2s

      /* Animation */
      &:hover
        transform: scale(1.3)

  @media (min-width: 1200px)
    padding: 0rem 1rem

    .nav-item
      padding: 0.5rem 0.24rem
</style>





