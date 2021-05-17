# Vue - Day 2 Practice

- Youtube Project

```vue
vue create my-youtube-project

cd my-youtube-project

npm i lodash

npm run serve
```

```vue
// src - componenets - SearchBar.vue

<template>
  <div>
    
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

```vue
// src - App.vue

<template>
  <div id="app">
    <h1>My Youtube Project</h1>
    <SearchBar/>
  </div>
</template>

<script>
import SearchBar from '@/components/SearchBar.vue'

export default {
  name: 'App',
  components: {
    SearchBar,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```



## 1. Youtube Project

### 1.1. Youtube Data V3 API

- event의 데이터가 어디에 있는 지 확인! : event.target.value
- emit 이벤트 이름은 kebab-case

```vue
// src - componenets - SearchBar.vue

<template>
  <div>
    <input type="text" @keyup.enter="onInputKeyword">
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  methods: {
    onInputKeyword: function (event) {
      // console.log(event.target.value)
      this.$emit('input-search', event.target.value)
    }
  }
}
</script>

<style>

</style>
```

- 데이터가 emit으로 잘 들어오는 지 확인

```vue
// src - App.vue

<template>
  <div id="app">
    <h1>My Youtube Project</h1>
    <SearchBar @input-search="onInputSearch"/>
  </div>
</template>

<script>
import SearchBar from '@/components/SearchBar.vue'

export default {
  name: 'App',
  components: {
    SearchBar,
  },
  data: function () {
    return {
      searchData: '',
    }
  },
  methods: {
    onInputSearch: function (searchData) {
      console.log('searchbar의 이벤트가 감지되었습니다!')
      console.log(searchData)
      this.searchData = searchData
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

- Youtube API KEY

```vue
// .env.local
VUE_APP_YOUBUE_API_KEY='AIzaSyBRBRAeobINPv1_D3twK5lI2-hbn0GUG64'

// App.vue
const API_KEY = process.env.VUE_APP_YOUBUE_API_KEY

// youtube data api
// https://developers.google.com/youtube/v3/docs/search
```



- Axios 설치 및 import
- Youtube로부터 Data 받아오기

```VUE
npm install axios
import axios from 'axios'


// axios 사용
<template>
  <div id="app">
    <h1>My Youtube Project</h1>
    <SearchBar @input-search="onInputSearch"/>
  </div>
</template>

<script>
import axios from 'axios'
import SearchBar from '@/components/SearchBar.vue'

const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
  name: 'App',
  components: {
    SearchBar,
  },
  data: function () {
    return {
      searchData: '',
      videos: [],
    }
  },
  methods: {
    onInputSearch: function (searchData) {
      // console.log('searchbar의 이벤트가 감지되었습니다!')
      // console.log(searchData)
      this.searchData = searchData

      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: this.searchData,
      }

      axios({
        url: API_URL,
        method: 'get',
        params,
      })
        .then(response => {
          // console.log(response)
          console.log(response.data.items)
          this.videos = response.data.items
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```



### 1.2. App - VideoList - VideoListItem

```vue
<template>
  <div id="app">
    <h1>My Youtube Project</h1>
    <SearchBar @input-search="onInputSearch"/>
    <!-- 왼쪽은 props를 받을 곳의 이름 // 오른쪽은 여기서 보내는 데이터 값 -->
    <VideoList :videos="videos"/>
  </div>
</template>

<script>
import axios from 'axios'
import SearchBar from '@/components/SearchBar.vue'
import VideoList from '@/components/VideoList.vue'


const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
  },
  data: function () {
    return {
      searchData: '',
      videos: [],
    }
  },
  methods: {
    onInputSearch: function (searchData) {
      // console.log('searchbar의 이벤트가 감지되었습니다!')
      // console.log(searchData)
      this.searchData = searchData

      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: this.searchData,
      }

      axios({
        url: API_URL,
        method: 'get',
        params,
      })
        .then(response => {
          // console.log(response)
          console.log(response.data.items)
          this.videos = response.data.items
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

```vue
// VideoList.vue

<template>
  <ul>
    <VideoListItem 
      v-for="(video, index) in videos" 
      :key="index"
      :video="video"
    />
  </ul>
</template>

<script>
import VideoListItem from '@/components/VideoListItem.vue'

export default {
  name: 'VideoList',
  components: {
    VideoListItem,
  },
  props: {
    // 상위에서 받은(props) 데이터를 상세하게 적습니다.
    videos: {
      type: Array,
    }
  }
}
</script>

<style>

</style>

```

- 이미지 URL은 미리 계산해둡니다 computued

```vue
// VideoListItem

<template>
  <li style="text-align: left;">
    <img :src="youtubeImgSrc" alt="img">
    {{ video.snippet.title }}
  </li>
</template>

<script>
export default {
  name: 'VideoListItem',
  props: {
    video: {
      type: Object,
    }
  },
  computed: {
    youtubeImgSrc: function () {
      return this.video.snippet.thumbnails.default.url
    }
  }
}
</script>

<style>

</style>
```

> unescaped 특수문자 처리
>
> - <span v-html="video.snippet.title"></span>
>
> - {{ video.snippet.title | stringUnescape }}
>
>   ```펻
>     import _ from 'lodash'
>     
>     ...
>     
>     filters: {
>       stringUnescape: function (rawText) {
>         return _.unescape(rawText)
>
>       }
>   ```



- Data를 emit으로 끌어 올려줍니다.

```vue
// VideoListItem

<template>
  <li @click="selectVideo" style="text-align: left;">
    <img :src="youtubeImgSrc" alt="img">
    <!-- <span v-html="video.snippet.title"></span> -->
    {{ video.snippet.title | stringUnescape }}
  </li>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'VideoListItem',
  props: {
    video: {
      type: Object,
    }
  },
  methods: {
    selectVideo: function () {
      this.$emit('select-video', this.video)
    }
  },
  computed: {
    youtubeImgSrc: function () {
      return this.video.snippet.thumbnails.default.url
    }
  },
  filters: {
    stringUnescape: function (rawText) {
      return _.unescape(rawText)

    }
  },
}
</script>

<style>

</style>
```

```vue
// VideoList

<template>
  <ul>
    <VideoListItem 
      v-for="(video, index) in videos" 
      :key="index"
      :video="video"
      @select-video="onSelectVideo"
    />
  </ul>
</template>

<script>
import VideoListItem from '@/components/VideoListItem.vue'

export default {
  name: 'VideoList',
  components: {
    VideoListItem,
  },
  props: {
    // 상위에서 받은(props) 데이터를 상세하게 적습니다.
    videos: {
      type: Array,
    }
  },
  methods: {
    onSelectVideo: function (video) {
      console.log(video)
      this.$emit('select-video', video)
    }
  }
}
</script>

<style>

</style>
```

```vue
// app

<template>
  <div id="app">
    <h1>My Youtube Project</h1>
    <SearchBar @input-search="onInputSearch"/>
    <!-- 왼쪽은 props를 받을 곳의 이름 // 오른쪽은 여기서 보내는 데이터 값 -->
    <VideoList 
      :videos="videos"
      @select-video="onSelectVideo"
    />
  </div>
</template>

<script>
import axios from 'axios'
import SearchBar from '@/components/SearchBar.vue'
import VideoList from '@/components/VideoList.vue'


const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
  },
  data: function () {
    return {
      searchData: '',
      videos: [],
      selectVideo: '',
    }
  },
  methods: {
    onSelectVideo: function (video) {
      console.log(video)
      this.selectVideo = video
    },
    onInputSearch: function (searchData) {
      // console.log('searchbar의 이벤트가 감지되었습니다!')
      // console.log(searchData)
      this.searchData = searchData

      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: this.searchData,
      }

      axios({
        url: API_URL,
        method: 'get',
        params,
      })
        .then(response => {
          // console.log(response)
          console.log(response.data.items)
          this.videos = response.data.items
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

​	

### 1.3. VideoDetail

```vue
// app

<VideoDetail :video="selectVideo"/>

import VideoDetail from '@/components/VideoDetail.vue'

VideoDetail
```

```vue
// VideoDetail
<template>
  <div style="text-align: center;">
    <iframe :src="videoUrl" frameborder="0"></iframe>
  </div>
</template>

<script>
export default {
  name: 'VideoDetail',
  props: {
    video: {
      // 처음 데이터가 없는 상황일때
      // [] empty String으로 가지고 있다가 -> Data가 들어오면 Object으로 받음
      type: [String, Object],
    }
  },
  computed: {
    videoUrl: function () {
      const videoId = this.video.id.videoId
      return `https://www.youtube.com/embed/${videoId}`
    }
  }
}
</script>

<style>

</style>
```

- 예외처리 : 처음 렌더링에서 video 데이터가 없는 경우

```vue
<template>
  <div v-if="video" style="text-align: center; margin-top: 2rem;">
    <iframe :src="videoUrl" frameborder="0"></iframe>
    <h2>{{ video.snippet.title | stringUnescape }}</h2>
    <p>{{ video.snippet.description | stringUnescape }}</p>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'VideoDetail',
  props: {
    video: {
      // 처음 데이터가 없는 상황일때
      // [] empty String으로 가지고 있다가 -> Data가 들어오면 Object으로 받음
      type: [String, Object],
    }
  },
  computed: {
    videoUrl: function () {
      const videoId = this.video.id.videoId
      return `https://www.youtube.com/embed/${videoId}`
    }
  },
  filters: {
    stringUnescape: function (rawText) {
      return _.unescape(rawText)
    }
  },
}
</script>

<style>

</style>
```

- 검색결과 가장 처음 데이터를 객체에 넣어줍니다.

```vue
<template>
  <div v-if="video" style="text-align: center; margin-top: 2rem;">
    <iframe :src="videoUrl" frameborder="0"></iframe>
    <h2>{{ video.snippet.title | stringUnescape }}</h2>
    <p>{{ video.snippet.description | stringUnescape }}</p>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'VideoDetail',
  props: {
    video: {
      // 처음 데이터가 없는 상황일때
      // [] empty String으로 가지고 있다가 -> Data가 들어오면 Object으로 받음
      type: [String, Object],
    }
  },
  computed: {
    videoUrl: function () {
      const videoId = this.video.id.videoId
      return `https://www.youtube.com/embed/${videoId}`
    }
  },
  filters: {
    stringUnescape: function (rawText) {
      return _.unescape(rawText)
    }
  },
}
</script>

<style>

</style>
```