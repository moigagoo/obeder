<template>
  <div class="menu">
    <menu-header></menu-header>
    <div class="content">
      <daily-menu v-for="date in sortedDates" :day="date"></daily-menu>
    </div>
  </div>
</template>

<script>
  import _ from 'lodash';
  import usersService from 'api/users';

  import Header from './Header';
  import DailyMenu from './Menu/DailyMenu';

  export default {
    components: {
      'menu-header': Header,
      'daily-menu': DailyMenu,
    },
    created() {
      const id = localStorage.getItem('user_uid');

      usersService
        .getMenus(id)
        .then(
          (menuDates) => {
            this.dates = menuDates;
          },
          error => error,
        );
    },
    name: 'menu',
    data() {
      return {
        dates: [],
      };
    },
    computed: {
      sortedDates() {
        return _.sortBy(this.dates, ['date']);
      },
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "../assets/styles/variables.css";

.content {
  max-width: var(--desktop-width);
  padding: 15px;
  margin: 0 auto;
}

@media (--desktop) {
  .content {
    padding: 30px 0;
  }
}
</style>
