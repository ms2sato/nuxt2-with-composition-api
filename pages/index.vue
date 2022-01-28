<template>
  <div>
    <div @click="change()">{{name}}</div>
    <h3>tasks</h3>
    <button @click="add()">追加</button>
    <div v-if="posts.length">
      <ul>
        <li v-for="post in posts" :key="post.id">
          {{post.title}}<button @click="remove(post.id)">削除</button>
        </li>
      </ul>
    </div>
    <div v-if="!posts.length">
      まだ一件もありません
    </div>
  </div>
</template>

<script>
import { defineComponent, reactive, ref } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const posts = reactive([]);
    const name = ref('abc');

    const change = () => {
      const now = Date.now()
      name.value = 'cde' + now; 
    };

    const add = () => {
      const now = Date.now();
      posts.push({ id: now, title: `test${now}` });
    };

    const remove = (id) => {
      posts.splice(posts.findIndex((post) => post.id === id), 1)
    };

    return {name, change, add, posts, remove};
  }
})
</script>
