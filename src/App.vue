<template>
  <navbar :pages="pages" :active-page="activePage"></navbar>

  <!-- v-if="pages.length > 0" / v-show="pages.length > 0" -->
  <page-viewer :page="pages[activePage]"></page-viewer>

  <create-page @child-created="fatherCreated"></create-page>
</template>

<script>
import Navbar from "./components/Navber.vue";
import PageViewer from "./components/PageViewer.vue";
import CreatePage from "./components/CreatePage.vue";

export default {
  name: "App",
  components: {
    Navbar,
    PageViewer,
    CreatePage,
  },
  created() {
    this.getPages();

    this.$bus.$on("navbarLinkActived", (index) => {
      this.activePage = index;
    });
  },
  data() {
    return {
      activePage: 0,
      pages: [],
    };
  },
  methods: {
    async getPages() {
      let res = await fetch("pages.json");
      let data = await res.json();

      this.pages = data;
    },
    fatherCreated(pageObj) {
      console.log(pageObj);
      this.pages.push(pageObj);
    },
  },
};
</script>
