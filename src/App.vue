<script>
const API_URL = `https://api.github.com/repos/vuejs/core/commits?per_page=3&sha=`

export default {
  data: () => ({
    branches: ['main', 'v2-compat'],
    currentBranch: 'main',
    commits: null
  }),

  created() {
    // 초기화 시 가져오기
    this.fetchData()
  },

  watch: {
    // currentBranch가 변경될 때마다 데이터 다시 가져오기
    currentBranch: 'fetchData'
  },

  methods: {
    async fetchData() {
      const url = `${API_URL}${this.currentBranch}`
      this.commits = await (await fetch(url)).json()
    },
    formatDate(v) {
      return v.replace(/T|Z/g, ' ')
    }
  }
}
</script>

<template>
  <h1>Vue Core 최신 커밋</h1>
    <template v-for="branch in branches">
    <input type="radio" 
      :id="branch"
      :value="branch"
      name="branch"
      v-model="currentBranch">
    <label :for="branch">{{ branch }}</label>
  </template>
  <p>vuejs/vue@{{ currentBranch }}</p>
  <ul>
    <li v-for="{ html_url, sha, author, commit } in commits">
      <a :href="html_url" target="_blank" class="commit">{{ sha.slice(0, 7) }}</a>
      - <span class="author">
        <a :href="author.html_url" target="_blank">{{ commit.author.name }}</a>
      </span>
      at <span class="date">{{ formatDate(commit.author.date) }}</span>
    </li>
  </ul>
</template>

<style>
a {
  text-decoration: none;
  color: #42b883;
}
li {
  line-height: 1.5em;
  margin-bottom: 20px;
}
.author,
.date {
  font-weight: bold;
}
</style>

<!-- <script>
import { marked } from 'marked'
import { debounce } from 'lodash-es'

export default {
  data: ()=> ({
    input: '#반가워요!'
  }),
  computed:{
    output(){
      return marked(this.input)
    }
  },
  methods:{
    update: debounce(function (e) {
      this.input = e.target.value
    }, 100)
  }
}
</script>

<template>
  <div class="editor">
    <textarea class="input" :value="input" @input="update"></textarea>
    <div class="output" v-html="output"></div>
  </div>
</template>

<style>
body {
  margin: 0;
}

.editor {
  height: 100vh;
  display: flex;
}

.input,
.output {
  overflow: auto;
  width: 50%;
  height: 100%;
  box-sizing: border-box;
  padding: 0 20px;
}

.input{
  border: none;
  border-right: 1px solid #ccc;
  resize: none;
  outline: none;
  background-color: #f6f6f6;
  font-size: 14px;
  font-family: 'Monaco', courier, monospace;
  padding: 20px;
}

code {
  color: #f66;
}
</style> -->
