<!-- Layout for our event components. Contains the navigation for each event page & the API call for fetching the event -->
<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <div id="nav">
      <router-link :to="{ name: 'EventDetails' }">Details</router-link> | 
      <router-link :to="{ name: 'EventRegister' }">Register</router-link> | 
      <router-link :to="{ name: 'EventEdit' }">Edit</router-link>
    </div>
    <router-view :event="event" /> <!-- This is where the child components will be displayed on the screen -->
  </div>
</template>

<script>
import EventService from "@/services/EventService.js";

export default {
  props: ["id"],
  data() {
    return {
      event: null
    };
  },
  created() {
    //fetch event (by id) and set local data
    EventService.getEvent(this.id)
      .then(response => {
        this.event = response.data;
      })
      .catch(error => {
        if(error.response && error.response == 404){
          this.$router.push({
            name: '404Resource',
            params: { resource: 'event' }
          })
        } else {
          this.$router.push({ name: 'NetworkError' })
        }

      });
  }
};
</script>
