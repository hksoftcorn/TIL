# Vue - Day 3 Practice

- props & emit
- vuex

## 1. Youtube Vuex

### 1.1. Youtube API

```vue\
vue create youtube-vuex
vue add vuex
npm i
npm i lodash
npm install --save axios
```

```vue
// TheSearchBar.vue

<template>
  <div>
    <input type="text" @keyup.enter="onSearch" v-model="query">
    <button @click="onSearch">Search</button>
  </div>
</template>

<script>

export default {
  name: 'TheSearchBar',
  data: function () {
    return {
      query: ''
    }
  },
  methods: {
    onSearch: function () {
      this.$store.dispatch('onSearch', this.query)
    }
  }
}
</script>

<style>

</style>
```

```javascript
// index.js

import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'

Vue.use(Vuex)

const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default new Vuex.Store({
  state: {
    searchResult: null
  },
  mutations: {
    ADD_SEARCH_RESULT: function (state, searchResult) {
      state.searchResult = searchResult
    }
  },
  actions: {
    onSearch: function ({ commit }, query) {
      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: query,
      }
      axios ({
        url: API_URL,
        method: 'get',
        params,
      })
        .then(response => {
          commit('ADD_SEARCH_RESULT', response.data.items)
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  modules: {
  }
})

```

- .env.local

  `VUE_APP_YOUTUBE_API_KEY='AIzaSyBRBRAeobINPv1_D3twK5lI2-hbn0GUG64'`

### 1.2. Videos

```vue
<template>
  <ul>
    <h1>Video List</h1>
    <VideoListItem 
                   v-for="(video, idx) in videoList"
                   :key="idx"
                   :video="video"
    />
  </ul>
</template>

<script>
import VideoListItem from '@/components/VideoListItem'

export default {
  name: 'VideoList',
	components: {
    VideoListItem,
  },
  computed: {
    videoList: function () {
      return this.$store.state.searchResult
    }
  }
}
</script>

<style>

</style>

```

```vue
<template>
  <li @click="selectVideo(video)" style="text-align: left;">
    <img :src="videoImgSrc" alt="thumbnail">
		<span>{{ videoTitle }}</span>
  </li>
</template>

<script>
import _ from 'lodash'
import { mapActions } from 'vuex'

export default {
  name: 'VideoListItem',
  props: {
    video: {
      type: Object,
    }
  },
  methods: {
    ...mapActions([
      'selectVideo'
    ])
    // selectVideo: function () {
    //   this.$store.dispatch('selectVideo', this.video)
    // }
  },
  computed: {
    videoTitle: function () {
      return _.unescape(this.video.snippet.title)
    },
    videoImgSrc: function () {
      return this.video.snippet.thumbnails.default.url
    }
  }
}
</script>

<style>

</style>

```

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
import _ from 'lodash'

Vue.use(Vuex)

const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default new Vuex.Store({
  state: {
    searchResult: null,
    selectedVideo: null,
  },
  mutations: {
    ADD_SEARCH_RESULT: function (state, searchResult) {
      state.searchResult = searchResult
    }, 
    SELECT_VIDEO: function (state, video) {
      state.selectedVideo = video
    }
  },
  actions: {
    onSearch: function ({ commit }, query) {
      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: query,
      }
      axios ({
        url: API_URL,
        method: 'get',
        params,
      })
        .then(response => {
          commit('ADD_SEARCH_RESULT', response.data.items)
        })
        .catch(error => {
          console.log(error)
        })
    },
    selectVideo: function ({ commit }, video) {
      commit('SELECT_VIDEO', video)
    }
  },
  getters: {
    videoUrl: function (state) {
      if (state.selectedVideo) {
        const videoId = state.selectedVideo.id.videoId
        return `https://www.youtube.com/embed/${videoId}`
      }
      return null
    },
    videoTitle: function (state) {
      if (state.selectedVideo) {
        const title = state.selectedVideo.snippet.title
        return _.unescape(title)
      }
      return null
    },
    videoDesc: function (state) {
      if (state.selectedVideo) {
        const description = state.selectedVideo.snippet.description
        return _.unescape(description)
      }
      return null
    },
    isSelected: function (state) {
      return state.selectedVideo !== null
    }
  },
  modules: {
  }
})

```

```vue
<template>
  <div v-if="isSelected" style="margin-top: 25px;">
    <iframe :src="videoUrl" frameborder="1"></iframe>
    <h2>{{ videoTitle }}</h2>
    <p>{{ videoDesc }}</p>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'VideoDetail',
  computed: {
    ...mapGetters([
      'videoUrl',
      'videoTitle',
      'videoDesc',
      'isSelected'
    ])
  }
}
</script>

<style>

</style>

```



