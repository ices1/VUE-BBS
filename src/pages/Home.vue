<template>
    <div>
        <ul>
          <li class='post' v-for="item of posts" :key="item.id">
            <router-link :to="'post/' + item.id">
              <h3 class="title"> {{ item.title }} </h3>
              <p class="content"> {{ item.content }} </p>
            </router-link>
            <div class="user-info">
              <router-link :to="'user/' + item.userId">
                <img class="avatar" :src='"/api/avatars/" + item.avatar' alt="">
                <span class="post-username"> {{ item.username }} </span>
              </router-link>
              <span class="float-right"> {{ idTime(item.timestamp) }}</span>
            </div>
          </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'
import {indexTime} from '@/assets/js/common.js'

export default {
  name: 'Home',
  data () {
    return {
      posts: []
    }
  },
  mounted () {
    axios.defaults.withCredentials = true
    this.getInfo()
  },
  methods: {
    getInfo () {
      axios.get('/api/posts').then(this.getPosts)
    },
    getPosts (res) {
      if (res.status === 200) {
        // console.log(res)
        this.posts = res.data.data.posts
      } else {
        console.log(res)
      }
    },
    idTime (t) {
      return indexTime(t)
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '~styles/mixins.styl'

  .title
    font-size: .5rem
    padding-bottom: .2rem
    ellipsis()
  .content
    ellipsis()
</style>
