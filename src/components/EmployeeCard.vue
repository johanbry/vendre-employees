<script setup lang="ts">
interface IProps {
  first_name: string;
  last_name: string;
  email: string;
  avatar: string;
}
defineProps<IProps>();
</script>

<template>
  <article>
    <div class="profile-pic-container">
      <img :src="avatar" :alt="first_name + ' ' + last_name" />
    </div>
    <h3>{{ first_name }} {{ last_name }}</h3>
    <div>
      <a :href="'mailto:' + email">
        <font-awesome-icon icon="fa-solid fa-envelope" /> Skicka e-post</a
      >
    </div>
  </article>
</template>

<style scoped>
article {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 280px;
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 1rem;
  text-align: center;
  color: black;
}

img {
  border-radius: 50%;
  object-fit: cover;
  object-position: center;
  height: 100%;
  width: 100%;
}

.profile-pic-container {
  height: 125px;
  width: 125px;
  margin-bottom: 1rem;
}

h3 {
  font-weight: bold;
}
</style>
