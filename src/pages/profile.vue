<template>
  <q-page padding class="flex justify-center">
    <!-- content -->
    <q-card class="character-card" @click="alert = true">
      <img class="character-image" :src="character.img" :alt="character.name">
      <q-card-section>
        <div class="text-h6">{{character.name}}</div>
        <div class="text-subtitle2">{{character.nickname}}</div>
      </q-card-section>
       <q-card-section>
         <q-chip v-for="occupation in character.occupation"
          :key="occupation" color="teal" text-color="white" icon="bookmark">
           {{occupation}}
         </q-chip>
      </q-card-section>
    </q-card>

    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Birthday Info</div>
        </q-card-section>
        <q-card-section>{{character.birthday}}</q-card-section>
        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup></q-btn>
        </q-card-actions>
      </q-card>
    </q-dialog>


  </q-page>
</template>

<script>
import { QSpinnerCube } from 'quasar';
export default {
  async created() {

    this.$q.loading.show({
      message: 'Loading...',
      spinnerColor: 'blue',
      spinner: QSpinnerCube
    });
    const url = (this.$route.params.id === 1 || this.$route.params.id === 2) ?
     'https://www.breakingbadapi.com/api/characters/' : 'https://www.breakingbadapi.com/api/character/';
    const character = await this.$axios.get(url + this.$route.params.id);
    this.$q.loading.hide();
    this.character = character.data[0];
  },
  data() {
    return {
      character: {},
      alert: false
    }
  }
}
</script>

<style scoped>
  .character-card {
    width: 20%;
    /* max-width: 350px; */
  }
  .character-image {
    /* height: 400px;
    width: ; */
  }
</style>
