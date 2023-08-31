<template>
  <nav
    :class="[`navbar-${theme}`, `bg-${theme}`, 'navbar', 'navbar-expand-lg']"
  >
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Navbar</a>
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        >
        <!-- <a
            class="nav-link"
            aria-current="page"
            v-bind:class="{ active: activePage == index }"
            v-bind:href="page.link.url"
            v-bind:title="`This Link goes to the ${page.link.text} page`"
            v-on:click.prevent="navLinkClick(index)"
            >{{ page.link.text }}</a
          > -->
        <navbar-link
          v-for="(page, index) in publishedPages"
          class="nav-item"
          :key="index"
          :page="page"
          :index="index"
          :isActive="activePage == index"
          @actived="$emit('actived')"
        >
        </navbar-link>
      </ul>
      <form class="d-flex">
        <button class="btn btn-primary" @click.prevent="changTheme()">
          Toggle Nav
        </button>
      </form>
    </div>
  </nav>
</template>

<script>
import NavbarLink from "./NavbarLink.vue";

export default {
  components: {
    NavbarLink,
  },
  created() {
    this.getThemeSetting();
  },
  computed: {
    publishedPages() {
      return this.pages.filter((p) => p.published);
    },
  },
  props: ["pages", "activePage"],
  data() {
    return {
      theme: "dark",
    };
  },
  methods: {
    changTheme() {
      let theme = "light";
      if (this.theme == "light") {
        theme = "dark";
      }
      this.theme = theme;
      this.storeThemeSetting();
    },
    storeThemeSetting() {
      localStorage.setItem("theme", this.theme);
    },
    getThemeSetting() {
      let theme = localStorage.setItem("theme", this.theme);

      if (theme) {
        this.theme = theme;
      }
    },
  },
};
</script>
