
<template>
  <div class="hello">
    <form @submit.prevent="insert">
      <p><input type="text" name="name" v-model="name" /></p>
      <p><input type="submit" value="저장" /></p>
    </form>
  </div>

  <ul>
    <li v-for="item in data" :key="item.date">
      {{ item.name }} / {{ item.count }}
      <button @click="update(item.date, item.count)">좋아용</button>
      <button @click="del(item.date)">삭제</button>
    </li>
  </ul>
</template>

<script>
import axios from 'axios';
export default {

  //useEffect와 같은 놈
  mounted() {
    this.select();
  },
  methods: {
    select() {
      axios.get("http://localhost:3030/api")
        .then(res => {
          this.data = res.data;
        })
    },
    update(date, count) {
      axios.put(`http://localhost:3030/api/update?date=${date}`, { count: count + 1 })
        .then(res => {
          this.data = res.data;
        })
    },

    insert() {
      const data = { name: this.name, date: Date.now(), count: 0 }
      axios.post("http://localhost:3030/api/insert", data)
        .then(res => {
          this.data = res.data;
        })
    },
    del(date) {
      axios.delete(`http://localhost:3030/api/delete?date=${date}`)
        .then(res => {
          this.data = res.data;
        })
    }
  },
  data() {
    return { name: '', data: [] }
  },
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>


