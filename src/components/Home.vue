<template>
  <v-app theme="light">
    <v-layout>
      <v-navigation-drawer
        expand-on-hover
        rail
        elevation="2"
      >
        <v-list>
          <v-list-item
            prepend-avatar="https://i0.wp.com/taliopharowords.com/wp-content/uploads/2023/08/Todoface1.jpg?resize=768%2C828&ssl=1"
            subtitle="matheus_teste@gmailcom"
            title="Matheus Gonçalves"
          ></v-list-item>
        </v-list>

        <v-list density="comfortable" nav>
          <v-list-item @click="irParaCalc" prepend-icon="mdi-water-plus" title="HIDRACALC" value="myfiles"></v-list-item>
          <v-list-item prepend-icon="mdi-arm-flex" title="TREINOS" value="shared"></v-list-item>
          <v-list-item prepend-icon="mdi-star" title="Starred" value="starred"></v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-main style="height: 250px"></v-main>
    </v-layout>
  </v-app>
</template>


<script>


export default {

  // eslint-disable-next-line vue/multi-word-component-names
  name: "Home",
  data() {
    return {}
  },
  methods: {
    irParaCalc() {
      this.$router.push({ name: 'Calc' });
    }
  }
};
</script>

<style>
body {
  background-color: #8c8b8b;
}
</style>
