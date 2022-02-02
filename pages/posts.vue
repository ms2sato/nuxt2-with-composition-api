<template>
  <div>
    <h3>tasks</h3>
    <button @click="add()">追加</button>
    <button @click="reload()">再取得</button>
    <div v-if="!isLoading">
      <ul>
        <li v-for="post in posts" :key="post.id">
          {{post.title}}<button @click="remove(post.id)">削除</button>
        </li>
      </ul>
    </div>
    <div v-if="isLoading">
      読み込み中です
    </div>
  </div>
</template>

<script>
import { defineComponent, reactive, useFetch, computed } from '@vue/composition-api'
import axios from 'axios'

export default defineComponent({
  async asyncData({params}) {
    const res = await axios.get('http://localhost:3000/api/posts/1.json');
    const {posts} = res.data;
    return { posts, isLoading: false };
  },

  methods: {
    add: function () {
      const now = Date.now();
      console.log('add:this.posts', this.posts);
      this.posts.push({ id: now, title: `test${now}` });
    },
    remove: function (id) {
      this.posts.splice(this.posts.findIndex((post) => post.id === id), 1)
    }
  },

  setup(props) {
    console.log('setup:props', props);
    const posts = reactive([]);
    console.log('setup:posts', posts);
    return { posts };
  }
})
</script>
