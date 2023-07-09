<script setup lang="ts">
import { ref, watch } from "vue";
import { useRoute, useRouter } from "vue-router";
import EmployeeCard from "@/components/EmployeeCard.vue";

type TEmployee = {
  id: number;
  email: string;
  first_name: string;
  last_name: string;
  avatar: string;
};

const API_URL = `https://reqres.in/api/users`;
let isLoading = false;
let currentPage: number;
let totalPages: number;
const employeesList = ref<TEmployee[]>([]);

const router = useRouter();
const route = useRoute();

const fetchData = async () => {
  try {
    isLoading = true;
    const res = await fetch(
      `${API_URL}?page=${!currentPage ? (currentPage = 1) : currentPage}`
    );
    if (!res.ok) throw new Error(res.status.toString());
    const data = await res.json();
    employeesList.value = data.data;
    totalPages = data.total_pages;
    isLoading = false;
  } catch (error) {
    console.log("Error fetching data: " + error);
  }
};

const changePage = (newPage: number) => {
  router.push({ path: "", query: { page: newPage } });
};

watch(
  () => route.query.page,
  (newPage) => {
    currentPage = newPage ? +newPage : 1;
    fetchData();
  },
  { immediate: true }
);
</script>

<template>
  <main>
    <section class="employees-list">
      <EmployeeCard
        v-for="employee in employeesList"
        :key="employee.id"
        :first_name="employee.first_name"
        :last_name="employee.last_name"
        :email="employee.email"
        :avatar="employee.avatar"
      />
    </section>

    <button v-for="n in totalPages" :key="n" @click="changePage(n)">
      Sida {{ n }}
    </button>
  </main>
</template>
