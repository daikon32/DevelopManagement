<template>
  <div class="container">
    <div v-for="item in navObj" :key="item" class="nav-container">
      <ul class="navbar">
        <li>
          <router-link :to="item.to">
            {{ item.name }}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Navigation",
  data() {
    return {
      navObj: [
        { name: "ホーム", to: "/" },
        { name: "進捗管理", to: "/progressManagement" },
        { name: "資料", to: "/document" },
        { name: "ＦＡＱ", to: "/faq" },
        { name: "問い合わせ", to: "/inquiry" },
      ],
    };
  },
});
</script>

<style scoped lang="scss">
.container {
  // margin: 0 auto;
  // padding-left: 10px;
  // padding-left: 20px;
  // padding-right: 20px;
  // max-width: 1000px;
  background: #dd0dd6;
  display: flex;
}

.nav-container {
  padding-right: 10px;
}

.navbar {
  list-style-type: none;
  text-align: center;
  display: block;
}
</style>
