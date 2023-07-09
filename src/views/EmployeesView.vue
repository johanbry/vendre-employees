<script setup lang="ts">
import { ref } from "vue";

type TEmployee = {
  id: number;
  email: string;
  first_name: string;
  last_name: string;
  avatar: string;
};

const API_URL = `https://reqres.in/api/users/fr`;
let isLoading = false;
let totalPages: number;
const employeesList = ref<TEmployee[]>([]);

const fetchData = async () => {
  try {
    isLoading = true;
    const res = await fetch(`${API_URL}`);
    if (!res.ok) throw new Error(res.status.toString());
    const data = await res.json();
    employeesList.value = data.data;
    totalPages = data.total_pages;
    isLoading = false;
  } catch (error) {
    console.log("Error fetching data: " + error);
  }
};
fetchData();
</script>

<template>
  <main>
    {{ employeesList }}<br />
    {{ totalPages }}<br />
  </main>
</template>
