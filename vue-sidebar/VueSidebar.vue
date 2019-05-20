<template>
  <div class="vue-sidebar" :class="{'open': full}">

    <div class="vue-sidebar-toggle cursor-pointer" @click="toggleFull()">
      <i class="fal fa-times fa-1x5" v-if="full"></i>
      <i :class="`${toggleIcon ? toggleIcon : 'fal fa-bars fa-1x5'}`" v-else></i>
    </div>

    <div class="vue-sidebar-list">
      <div class="vue-sidebar-item" v-for="link in links" :class="{'active-item': isActive(link)}">
        <router-link :to="{'name': link.name, 'params': link.params}" class="vue-sidebar-link">
          <i :class="`${link.icon} ${full ? 'mr-2' : ''}`" class="vue-sidebar-icon"></i>
          <transition name="fade">
            <span v-if="fullTitle">{{ link.title }}</span>
          </transition>
        </router-link>
      </div>
    </div>

  </div>
</template>

<script>

  import style from './static/css/font-awesome.css'
  import fonts from './static/css/fonts.css'

  export default {
    name: "VueSidebar",

    props: ["links", "toggleIcon"],

    data() {
      return {
        full     : false,
        fullTitle: false
      }
    },

    methods: {
      isActive(link) {
        return (link.name ? link.name.toLowerCase() : '') === (this.$route.name ? this.$route.name.toLowerCase() : '')
      },

      toggleFull() {
        this.full = !this.full
        if (this.full)
          setTimeout(() => {
            this.fullTitle = true
          }, 400)
        else this.fullTitle = false

        this.$emit('collapse', this.full)
      }
    }
  }
</script>

<style scoped>

  .vue-sidebar {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    height: 100%;
    width: 50px;
    -webkit-transition: width .5s;
    -moz-transition: width .5s;
    -ms-transition: width .5s;
    -o-transition: width .5s;
    transition: width .5s;
    background-color: #EDDBCD;
    font-family: "Poppins", sans-serif;
  }

  .vue-sidebar.open {
    width: 250px;
  }

  .vue-sidebar .vue-sidebar-list .vue-sidebar-item {
    position: relative;
  }

  .vue-sidebar .vue-sidebar-list .vue-sidebar-item .vue-sidebar-link {
    position: relative;
    display: block;
    font-size: 16px;
    font-weight: 400;
    padding: 10px;
    line-height: 30px;
    text-decoration: none;
    z-index: 20;
    -webkit-transition: 0.3s all;
    transition: 0.3s all;
    color: inherit;
  }

  .vue-sidebar .vue-sidebar-list .vue-sidebar-item.active-item .vue-sidebar-link {
    -webkit-box-shadow: inset 3px 0 0 0 #212529;
    box-shadow: inset 3px 0 0 0 #212529;
  }

  .vue-sidebar.open .vue-sidebar-list .vue-sidebar-item.active-item .vue-sidebar-link {
    background-color: #212529;
    color: #fff;
  }

  .vue-sidebar .vue-sidebar-list .vue-sidebar-item:hover .vue-sidebar-link {
    background-color: #fff;
  }

  .vue-sidebar .vue-sidebar-icon {
    width: 30px;
    text-align: center;
  }

  .vue-sidebar .vue-sidebar-toggle {
    height: 30px;
    width: 50px;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 10px auto 30px auto;
  }

  .mr-2 {
    margin-right: 8px;
  }

  .fa-1x5 {
    font-size: 1.5em;
  }

  .cursor-pointer {
    cursor: pointer;
  }

</style>
