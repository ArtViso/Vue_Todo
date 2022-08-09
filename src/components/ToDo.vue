<template>
  <v-container>
    <v-card
        class="mx-auto"
        max-width="1000"
    >
      <v-img
          class="white--text"
          height="200px"
          style="background: linear-gradient(180deg, rgba(0,104,255,1) 0%, rgba(255,255,255,1) 100%);"
      >
        <v-card-title>
          <h1>TODO-LIST</h1>
          <v-img
              class="align-start"
              :src="require('../assets/logo.svg')"
              contain
              position="right"
              height="35"
          />
        </v-card-title>
        <v-card-title class="justify-center">
          <h2>{{ date }}</h2>
        </v-card-title>
      </v-img>

      <div class="d-flex pa-4" style="height: 90px">
        <v-text-field class="mr-2" v-model="text" label="Enter a new task" outlined
                      v-on:keyup.enter="createListTask"></v-text-field>
        <v-btn dark x-large color="blue" @click="createListTask" style="height: 55px">
          <v-icon>
            mdi-plus
          </v-icon>
          Add
        </v-btn>
      </div>

      <v-card-title>
        <h4>ToDo:</h4>
      </v-card-title>
      <v-card-subtitle>
        You have <b>{{ list.length }}</b> tasks
      </v-card-subtitle>

      <v-card-text class="text--primary">
        <div>
          <div v-if="!list.length">
            <h1 class="text-center blue--text text-uppercase">
              You don't have tasks
              <svg style="height:34px;vertical-align: text-top;" viewBox="0 0 24 24">
                <path fill="currentColor"
                      d="M20,12A8,8 0 0,0 12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12M22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2A10,10 0 0,1 22,12M15.5,8C16.3,8 17,8.7 17,9.5C17,10.3 16.3,11 15.5,11C14.7,11 14,10.3 14,9.5C14,8.7 14.7,8 15.5,8M10,9.5C10,10.3 9.3,11 8.5,11C7.7,11 7,10.3 7,9.5C7,8.7 7.7,8 8.5,8C9.3,8 10,8.7 10,9.5M12,14C13.75,14 15.29,14.72 16.19,15.81L14.77,17.23C14.32,16.5 13.25,16 12,16C10.75,16 9.68,16.5 9.23,17.23L7.81,15.81C8.71,14.72 10.25,14 12,14Z"/>
              </svg>
            </h1>
          </div>
          <v-col>
            <v-row
                align="center"
                v-for="(value, index) in list"
                :key="value.message"
            >
              <v-checkbox
                  hide-details
                  color="success"
                  class="shrink mr-2 mt-0"
                  v-model="value.status"
              ></v-checkbox>
              <v-text-field :value="value.text" :disabled="value.status" readonly></v-text-field>
              <v-btn class="pa-0" color="red" style="margin-left: 5px;" @click="deleteSelectedTask(index)" text>
                <v-icon large>
                  mdi-delete
                </v-icon>
              </v-btn>
            </v-row>
          </v-col>
        </div>
      </v-card-text>

      <v-card-actions class="justify-space-between pa-4">
        <v-btn color="blue" @click="deleteAllTask" text>
          Clear all
          <v-icon left>
            mdi-delete
          </v-icon>
        </v-btn>
        <div class="d-flex justify-space-around" style="gap: 20px">
          <div class="blue pa-2">
            <span class="white--text">SUCCESS: {{ onSuccess(list).length }}</span>
          </div>
          <div class="red darken-1 pa-2">
            <span class="white--text">PENDING: {{ onPending(list).length }}</span>
          </div>
        </div>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'ToDo',

  data: () => ({
    text: '',
    date: new Date().toLocaleDateString('en-US', {year: 'numeric', month: '2-digit', day: '2-digit'}),

    list: [],
  }),

  methods: {
    createListTask() {
      const newNote = {text: this.text, status: false}
      if (this.text !== '' && this.text.trim() !== '') {
        this.list.push(newNote);
      }
      this.text = '';
    },

    deleteSelectedTask(index) {
      this.$delete(this.list, index)
    },

    deleteAllTask() {
      this.list = [];
    },

    onSuccess(status){
      return status.filter(item => {
        return item.status === true;
      })
    },

    onPending(status){
      return status.filter(item => {
        return item.status === false;
      })
    }
  },

  mounted() {
    if (localStorage.list) {
      this.list = JSON.parse(localStorage.list);
    }
  },

  watch: {
    list: {
      handler(newNotes) {
        localStorage.list = JSON.stringify(newNotes);
      },
      deep: true
    }
  }
}
</script>

<style>
  input[disabled] {
    text-decoration: line-through;
    border: none;
  }
</style>
