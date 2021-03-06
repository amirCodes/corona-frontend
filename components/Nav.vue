<template>
  <nav v-sticky class="flex items-center flex-wrap bg-white text-primary">
    <div class="container py-2 px-2 items-center font-bold text-lg">
      <div class="w-full flex items-center justify-between -my-2">
        <logo class="lg:flex"/>
        <div class="flex ml-auto mr-1" style="font-size: 0.98rem;">
          <template v-for="link in links">
            <a
              v-if="link.url"
              :key="link.name"
              class="mx-1 px-3 py-4 hidden lg:block nav-link"
              :href="link.url"
            >
              {{ $t(link.display) }}
            </a>
            <nuxt-link
              v-else
              :key="link.name"
              class="mx-1 px-3 py-4 hidden lg:block nav-link"
              :to="localePath(link.name)"
            >
              {{ $t(link.display) }}
            </nuxt-link>
          </template>

          <LanguageSelector />
        </div>
        <!--div class="capitalize">{{ currentPageName }}</div-->
        <button
          id="hamburger"
          class="lg:hidden flex self-center items-center px-3 py-2 border rounded text-primary border-primary
          hover:text-red-600 hover:border-red-600 self-end"
          @click="showMobileLinks = !showMobileLinks"
          aria-label="Menu"
        >
          <svg class="fill-current h-3 w-3" viewBox="0 0 20 20">
            <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/>
          </svg>
        </button>
      </div>
      <template v-if="showMobileLinks">
        <template v-for="link in links">
          <a
            v-if="link.url"
            :key="link.name"
            class="py-2 block hover:text-red-600 text-center lg:hidden"
            :href="link.url"
          >
            {{ $t(link.display) }}
          </a>
          <nuxt-link
            v-else
            :key="`mob-${link.name}`"
            class="py-2 block hover:text-red-600 text-center lg:hidden"
            :to="localePath(link.name)"
          >
            {{ $t(link.display) }}
          </nuxt-link>
        </template>
      </template>
      <!---->
    </div>
  </nav>
</template>

<script>
  import Logo from '../components/Logo';
  import LanguageSelector from './LanguageSelector'

  export default {
    name: "Nav",
    components: {
      LanguageSelector,
      Logo
    },
    computed: {
      currentPageName: function() {
        const currentLink = this.links.find(link => link.name === this.$route.name);
        return currentLink && currentLink.display || '';
      }
    },
    data: function() {
      return {
        links: [
          { name: "index", display: 'menu.home' },
          { name: "travel-alert", display: "menu.travel_alert" },
          { name: "causes", display: "menu.whats_covid" },
          { name: "prevention", display: "menu.prevention" },
          { name: "analytics", display: "menu.analytics" },
          { name: "blog", display: "menu.blog", url: "https://www.coronatracker.com/blog/" },
          { name: "about", display: 'menu.about' }
        ],
        showMobileLinks: false,
      }
    }
  };
</script>

<style>
  .top-sticky {
    box-shadow: 0 -4px 6px 6px #e8e8e8;
  }

  nav {
    border-bottom: 1px solid #e2e8f0;
  }

  .nav-link {
    position: relative;
    transition: all ease-in-out .2s;
  }

  .nav-link:hover {
    text-shadow: 0 0 1px #108885;
  }

  .nav-link::after {
    content: '';
    position: absolute;
    width: 100%;
    height: 4px;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #108885;
    transform: scale(0);
    transform-origin: 50% 50%;
    transition: all ease-in-out .2s;
  }

  .nuxt-link-exact-active::after,
  .nav-link:hover::after {
    transform: scale(1);
  }
</style>
