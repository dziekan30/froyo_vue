<template>
      <div id="app">
        <div id="nav">
              <router-link to="/">Home</router-link>
              <router-link to="/signup">Signup</router-link>
              <router-link to="/login">Login</router-link>
              <router-link to="/logout">Logout</router-link>
              <router-link to="/postsindex">PostsIndex</router-link>
            </div>
            <router-view/>
        <div class="main">
          <div class="calendar-holder">
            <calendar :events="events" />
          </div>
          <div class="form-holder">
            <h3>Schedule an event</h3>
            <event-form />
          </div>
        </div>
      </div>
    </template>
    <script>
    import Calendar from './components/Calendar.vue'
    import EventForm from './components/EventForm.vue'
    import Pusher from 'pusher-js';

    export default {
      name: 'app',
      components: {
        Calendar,
        EventForm
      },
      data(){
        return {
          events: [{
            title     :  'event1',
            start     : '2018-07-09',
            cssClass  : 'blue',
            YOUR_DATA : {}
          },
          {
            title     : 'event2',
            start     : '2018-07-10',
            end       : '2018-07-13',
            cssClass  : ['orange']
          }] 
        }
      },
      created(){
        const pusher = new Pusher('ec60077d2b4f79664fd5', {
          cluster: 'us2',
          encrypted: true,
        });
        const channel = pusher.subscribe('schedule');
        channel.bind('new-event', (data) => {
          this.events = [
            this.events,
            data
          ];
        })
      }
    }
    </script>

    <style>
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }
    .main {
      display: flex;
      align-items: center;
    }
    .calendar-holder {
      width: 65%;
    }
    .form-holder {
      width: 35%;
    }
    .form-holder > h3 {
      color: orangered;
      text-transform: uppercase;
      font-size: 16px;
      text-align: left;
      margin-left: 30px;
      margin-bottom: 10px;
    }
    </style>