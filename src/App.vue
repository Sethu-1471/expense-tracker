<template>
  <v-app >
    <v-main>
      <v-row>
        <v-col cols="12" sm="11" md="11" class="layout align-center justify-center">
          <HelloWorld />
        </v-col>
        <v-col cols="12" sm="11" md="1" >
         <v-switch v-model="switch1" ></v-switch>
        </v-col>
      </v-row>
     
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld
  },

  data: () => ({
    switch1: false,
    //
  }),
  updated(){
    console.log(this.switch1);
    this.$vuetify.theme.dark = this.switch1;
  }
};
</script>

<style>
.redColor{
  color: #E65100;
}
.greenColor{
  color: #4CAF50;
}
</style>