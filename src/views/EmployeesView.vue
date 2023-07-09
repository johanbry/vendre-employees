<script setup lang="ts">
import { ref, watch } from "vue";
import { useRoute, useRouter } from "vue-router";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/css/index.css";
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
const fullPage = true;
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

const noResult = () => employeesList.value.length < 1 && !isLoading;

const onCancel = () => {};

watch(
  () => route.query.page,
  async (newPage) => {
    currentPage = newPage ? +newPage : 1;
    await fetchData();
    if (currentPage > totalPages && totalPages > 0) {
      currentPage = 1;
      fetchData();
    }
  },
  { immediate: true }
);
</script>

<template>
  <main>
    <h1>Medarbetare</h1>
    <p>Vår största tillgång hos Vendre är våra fantastiska medarbetare.</p>
    <section class="employees-list full-width">
      <Loading
        v-model:active="isLoading"
        :can-cancel="true"
        :on-cancel="onCancel"
        :is-full-page="fullPage"
        :loader="'spinner'"
        :opacity="0"
      />

      <div v-if="noResult()">Ingen data att visa...</div>
      <EmployeeCard
        v-for="employee in employeesList"
        :key="employee.id"
        :first_name="employee.first_name"
        :last_name="employee.last_name"
        :email="employee.email"
        :avatar="employee.avatar"
      />
    </section>

    <section class="pagination">
      <p>Visar sida {{ currentPage }} av {{ totalPages }}</p>
      <button v-for="n in totalPages" :key="n" @click="changePage(n)">
        Sida {{ n }}
      </button>
    </section>
  </main>
</template>

<style scoped>
main {
  text-align: center;
}
.employees-list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  list-style-type: none;
  gap: 3rem;
  background-image: linear-gradient(-45deg, #45a5ec, #5333ed, #45a5ec);
  padding: 3rem 3rem;
  margin: 3rem 0 2rem 0;
  color: #fff;
}

.full-width {
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
}

.pagination {
  font-size: 14px;
}

button {
  border: 2px solid #5333ed;
  border-radius: 4px;
  background-color: #fff;
  padding: 0.5rem 1rem;
  margin: 0.5rem;
  cursor: pointer;
  text-transform: uppercase;
  font-weight: bold;
  transition: background-color 0.5s, color 0.5s;
}

button:hover {
  background-color: #5333ed;
  color: white;
}

@media (min-width: 640px) {
  .employees-list {
    padding: 5rem 5rem;
  }
}
</style>
