<template>
  <div id="app">
    <h1>Science News</h1>
    <p>Find the latest discoveries here</p>

    <div class="columns is-mobile">
      <div class="column is-half is-offset-one-quarter">
    <div class="select">
      <select v-model="selected" v-on:change="flair(selected)">
        <option disabled value="">Select a science topic</option>
        <option value="astronomy+OR++flair%3Aastro">Astronomy</option>
        <option value="biology+OR+flair%3Abio">Biology</option>
        <option value="cancer">Cancer</option>
        <option value="chemistry+OR+flair%3Achem">Chemistry</option>
        <option value="%22Computer+Science%22+OR+flair%3Acompsci">Computer Science</option>
        <option value="engineering+OR+flair%3Aeng">Engineering</option>
        <option value="mathematics+OR+flair%3Amath">Mathematics</option>
        <option value="nanoscience+OR+flair%3Anano">Nano science</option>
        <option value="neuroscience+OR+flair%3Aneuro">Neuroscience</option>
        <option value="physics">Physics</option>
        <option value="psychology+OR+flair%3Apsych">Psychology</option>
        <option value="%22Social+Science%22+OR+flair%3Asoc">Social Sciences</option>
      </select>
    </div>

    <div class="main">
      <div class="box" v-for="item in todos" :key="item.id">
        <a v-bind:href="item.data.url" target="_blank">
          <article class="media">
            <div class="media-left">
              <figure class="image is-64x64">
                <img v-bind:src="item.data.thumbnail" width="128" />
              </figure>
            </div>
            <div class="media-content">
              <div class="content">
                <p>
                  <strong style="text-transform: uppercase;">{{ item.data.domain }}</strong> <small>{{ item.data.link_flair_text }}</small>
                  <br>
                  <span style="font-size: 13px;">{{ item.data.title }}</span>
                </p>
              </div>
              <nav class="level is-mobile">
                <div class="level-left">
                  <a class="level-item" aria-label="reply">
                    <span class="icon is-small">
                      <i class="fas fa-reply" aria-hidden="true"></i>
                    </span>
                  </a>
                  <a class="level-item" aria-label="retweet">
                    <span class="icon is-small">
                      <i class="fas fa-retweet" aria-hidden="true"></i>
                    </span>
                  </a>
                  <a class="level-item" aria-label="like">
                    <span class="icon is-small">
                      <i class="fas fa-heart" aria-hidden="true"></i>
                    </span>
                  </a>
                </div>
              </nav>
            </div>
          </article>
        </a>
      </div>
    </div>
  </div>
</div>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      queryFirst : 'https://www.reddit.com/r/science/.json',
      queryStart : 'https://www.reddit.com/r/science/search.json?q=flair%3A',
      queryLinkParams : '+-site%3A%27reddit.com%27&restrict_sr=on&sort=new&t=all',
      todos: [],
      selected: ''
    };
  },
  methods: {
    flair(value){
      let root = this;
      var res = axios.get(this.queryStart + value + this.queryLinkParams).then(function (response) {
        root.todos=response.data.data.children;
      });
      return res;
    }
  },
  async created() {
    try {
      var res = await axios.get(this.queryFirst);
      this.todos = res.data.data.children;
    } catch(e) {
      console.error(e);
    }
  }
};
</script>

<style lang="sass"> 
#app 
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50

h1 
  margin: 20px 0 0 0
  font-size: 60px
  font-weight: bold
  line-height: 1

.select 
  margin-bottom: 20px
  width: 100%

select
  width: 100%

</style>
