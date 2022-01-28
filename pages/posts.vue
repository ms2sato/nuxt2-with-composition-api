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
import { defineComponent, reactive, useFetch, computed } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const posts = reactive([]);
    const add = () => {
      const now = Date.now();
      posts.push({ id: now, title: `test${now}` });
    };

    const remove = (id) => {
      posts.splice(posts.findIndex((post) => post.id === id), 1)
    };

    // TODO: 確認すること。useFetchはこの形で使われているのか。Nuxt3では動作がちがうっぽい。
    // TODO: 確認すること。SSRは自動で？
    const {$fetch, $fetchState} = useFetch(async () => {
      const res = await fetch('http://localhost:3000/api/posts/1.json');
      const {posts: newPosts} = await res.json();
      
      console.log(newPosts);
      posts.splice(0, posts.length);
      for(const post of newPosts) {
        posts.push(post);
      }
    });

    const reload = () => {
      $fetch();
    }

    return {add, posts, remove, reload, isLoading: computed(() => $fetchState.pending)};
  }
})
</script>
