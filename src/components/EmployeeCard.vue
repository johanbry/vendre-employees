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
  {{ first_name }}
  {{ last_name }}
  {{ email }}
  {{ avatar }}
</template>
<style scoped></style>
