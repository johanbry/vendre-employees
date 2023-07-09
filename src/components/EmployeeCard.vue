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
    <a :href="'mailto:' + email">{{ email }}</a>
  </article>
</template>

<style scoped>
article {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 250px;
  background-color: #efefef;
  padding: 2rem;
  border-radius: 1rem;
  text-align: center;
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
