<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addDataInArray">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="becomeType">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <app-value-input
        label="Значение"
        v-model:value="becomeValue"
      ></app-value-input>

      <button class="btn primary" :disabled="becomeValue.length < 3">
        Добавить
      </button>
    </form>

    <div class="card card-w70">
      <div v-for="fData in resumeValues" :key="fData">
        <h1 v-if="fData.type === 'title'">{{ fData.value }}</h1>
        <div class="avatar" v-if="fData.type === 'avatar'">
          <img :src="fData.value" />
        </div>
        <h2 v-if="fData.type === 'subtitle'">{{ fData.value }}</h2>
        <p v-if="fData.type === 'text'">
          {{ fData.value }}
        </p>
      </div>

      <h3 v-if="resumeValues.length === 1">
        Добавьте первый блок, чтобы увидеть результат
      </h3>
    </div>
  </div>
  <div class="container">
    <p>
      <button class="btn primary" @click="getComments">
        Загрузить комментарии
      </button>
    </p>
    <div v-if="loading !== true">
      <div class="card" v-if="comments.length !== 0">
        <h2>Комментарии</h2>
        <ul class="list">
          <li class="list-item" v-for="comment in comments" :key="comment.id">
            <div>
              <p>
                <strong>{{ comment.email }}</strong>
              </p>
              <small>{{ comment.body }}</small>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div v-else class="loader"></div>
  </div>
</template>

<script>
//https://jsonplaceholder.typicode.com/comments?_limit=42
import AppValueInput from "./components/AppValueInput.vue";
import axios from "axios";
export default {
  components: { AppValueInput },

  data() {
    return {
      becomeType: "title",
      becomeValue: "",
      resumeValues: [
        {
          type: "",
          value: "",
        },
      ],
      comments: [],
      loading: false,
    };
  },
  methods: {
    addDataInArray() {
      this.resumeValues.push({
        type: this.becomeType,
        value: this.becomeValue,
      });
      this.becomeValue = '';
      this.becomeType = 'title';
    },
    getComments() {
      this.loading = true;
      setTimeout(async () => {
        const { data } = await axios.get(
          "https://jsonplaceholder.typicode.com/comments?_limit=42"
        );
        this.comments = data;
        this.loading = false;
      }, 500);
    },
  },
};
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
