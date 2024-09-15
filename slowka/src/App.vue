<script>
import FindIn4 from './components/FindIn4.vue';
import WriteIn from './components/WriteIn.vue';
export default {
  components: {
    FindIn4, WriteIn
  },
  data() {
    return {
      method: 'pl',
      progress: 0,
      msg: null,
      msgClass: null
    }
  },
  methods: {
    changeView: function (lang) {
      this.method = lang;
    },
    addPoint: function (points) {
      this.progress += points;

      if(this.progress == 100){
        this.showAlert('success', 'Gratulacje! Udało Ci się!');
        this.progress = 0;
      }
    },
    showAlert: function (status, msg) {
      this.msgClass = status;
      this.msg = msg;

      setTimeout(() => {
        this.msg = null;
      }, 700);
    }
  }
}

</script>

<template>
  <header>
    <ul class="nav nav-tabs justify-content-center">
      <li class="nav-item">
        <a class="nav-link" :class="{active: method=='pl'}" aria-current="page" href="#" @click="changeView('pl')">znajdź
          polski</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" :class="{active: method=='en'}" aria-current="page" href="#" @click="changeView('en')">znajdź
          angielski</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" :class="{active: method=='plw'}" aria-current="page" href="#" @click="changeView('plw')">wpisz
          polski</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" :class="{active: method=='enw'}" aria-current="page" href="#" @click="changeView('enw')">wpis
          angielski</a>
      </li>
    </ul>
  </header>
  <main>
    <find-in4 lang="pl" v-if="method=='pl'"/>
    <find-in4 lang="en" v-if="method=='en'"/>
    <write-in lang="pl" v-if="method=='plw'"/>
    <write-in lang="en" v-if="method=='enw'"/>

    <div id="status" v-if="msg != null">
      <h2 class="alert text-center" :class="'alert-'+msgClass">
        {{msg}}
      </h2>
    </div>
  </main>
  <footer>
    <div class="progress" role="progressbar" aria-label="" :aria-valuenow="progress" aria-valuemin="0"
         aria-valuemax="100">
      <div class="progress-bar" :style="{width: progress+'%'}">{{ progress }}%</div>
    </div>
  </footer>
</template>

<style scoped>
#status {
  position: absolute;
  top: calc(50vh - 40px);
  left: 0;
  right: 0;
  width: 100%;
}
</style>
