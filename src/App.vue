<template>
  <div>
    <component :is="layout">
      <router-view />
    </component>
  </div>
</template>

<script>
import LayoutLogin from '@/layouts/LayoutLogin.vue';
import LayoutDefault from '@/layouts/LayoutDefault.vue';

export default {
  name: 'app',
  components: {
    LayoutLogin,
    LayoutDefault,
  },
  computed: {
    layout() {
      // В роутере проверяем св-во layout и переключаем слои
      if (this.$route.meta.layout === 'LayoutLogin') {
        return 'LayoutLogin';
      }
      return 'LayoutDefault';
    },
  },
};
</script>

<style lang="scss">
  @import "assets/zero.css";

  @import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,300;0,400;0,500;0,700;0,800;1,400&family=Roboto:wght@300;400;700;900&display=swap");

  #app {
    font-family: "Montserrat", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .container {
    padding: 50px;
  }

  .name-title{
    font-weight: 300;
    font-size: 36px;
    @media (max-width: 500px) {
      font-size: 26px;
    }
  }
  .description{
    font-weight: 300;
    font-size: 24px;
    line-height: 28px;

    font-family: "Roboto", sans-serif;
  }

  .d-flex{
    width: 100%;
    display: flex;
  }
  .gap-1{
    gap: 8px;
  }
  .gap-2{
    gap: 16px;
  }
  .gap-3{
    gap: 24px;
  }
  .gap-4{
    gap: 32px;
  }
  .flex-column{
    flex-direction: column;
  }
  .flex-center{
    justify-content: center;
    align-items: center;
  }
  .align-center{
    align-items: center;
  }

</style>
