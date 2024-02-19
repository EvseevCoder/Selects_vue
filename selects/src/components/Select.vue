<template>
  <div class="container">
    <h2>Работа с зависимыми Select</h2>
    <h3>Город</h3>
    <select v-model="selectedCity">
      <option disabled value="">Выберите город</option>
      <option v-for="(value, name) in cities" :key="name" :value="name">
        {{ name }}
      </option>
    </select>

    <h3>Цех</h3>
    <select v-model="selectedCeh">
      <option disabled value="">Выберите цех</option>
      <option v-for="ceh in filteredCehs" :key="ceh" :value="ceh">
        {{ ceh }}
      </option>
    </select>

    <h3>Сотрудник</h3>
    <select v-model="selectedWorker">
      <option disabled value="">Выберите сотрудника</option>
      <option v-for="worker in filteredWorkers" :key="worker" :value="worker">
        {{ worker }}
      </option>
    </select>

    <h3>Бригада</h3>
    <select v-model="selectedBr">
      <option disabled value="">Выберите бригаду</option>
      <option v-for="br in brigades" :key="br" :value="br">{{ br }}</option>
    </select>

    <h3>Смена</h3>
    <select v-model="selectedSm">
      <option disabled value="">Выберите смену</option>
      <option v-for="sm in shifts" :key="sm" :value="sm">{{ sm }}</option>
    </select>

    <button @click="saveData">Сохранить данные в Cookies</button>
    <button @click="testCookies">Проверка Cookies</button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const cities = ref({
  Москва: ["Цех_москва1", "Цех_москва2", "Цех_москва3"],
  Самара: ["Цех_самара1", "Цех_самара2", "Цех_самара3"],
  Питер: ["Цех_питер1", "Цех_питер2", "Цех_питер3"],
});

const workers = ref({
  Цех_москва1: ["Артем", "Иван"],
  Цех_москва2: ["Рашид", "Артур"],
  Цех_москва3: ["Антон", "Павел"],
  Цех_самара1: ["Игорь", "Вова"],
  Цех_самара2: ["Перт", "Владимир"],
  Цех_самара3: ["Вячеслав", "Денис"],
  Цех_питер1: ["Виктор", "Святослав"],
  Цех_питер2: ["Тимофей", "Дмитрий"],
  Цех_питер3: ["Глеб", "Игнат"],
});

const selectedCity = ref("");
const selectedCeh = ref("");
const selectedWorker = ref("");
const selectedBr = ref("");
const selectedSm = ref("");

const allCehs = computed(() => {
  const cehsSet = new Set();
  for (const cityCehs of Object.values(cities.value)) {
    cityCehs.forEach((ceh) => cehsSet.add(ceh));
  }
  return Array.from(cehsSet);
});

const filteredCehs = computed(() => {
  if (!selectedCity.value) {
    return allCehs.value; // Возвращаем все возможные цеха, если город не выбран
  }
  return cities.value[selectedCity.value] || [];
});

const filteredWorkers = computed(() => {
  if (!selectedCeh.value) return [];
  return workers.value[selectedCeh.value] || [];
});

const brigades = ["Бригада 1", "Бригада 2", "Бригада 3", "Бригада 4"];
const shifts = ["Смена 1", "Смена 2", "Смена 3", "Смена 4"];

function saveData() {
  let finalCity = selectedCity.value;

  // Если город не выбран, но выбран цех, ищем город для этого цеха
  if (!finalCity && selectedCeh.value) {
    for (const [city, cehs] of Object.entries(cities.value)) {
      if (cehs.includes(selectedCeh.value)) {
        finalCity = city;
        break; // Выходим из цикла, как только нашли соответствующий город
      }
    }
  }

  const data = {
    city: finalCity,
    ceh: selectedCeh.value,
    worker: selectedWorker.value,
    br: selectedBr.value,
    sm: selectedSm.value,
  };
  console.log("Куки сохранены", data);
  document.cookie = "userData=" + JSON.stringify(data) + ";path=/";
}

function testCookies() {
  alert(document.cookie);
}
</script>

<style scoped>
.container {
  padding: 20px;
  min-height: 100vh;
  width: 300px;
  /* max-width: 400px; */
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

h2,
h3 {
  color: #333;
}

select,
button {
  width: 100%;
  padding: 10px 15px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

select:hover,
select:focus,
button:hover,
button:focus {
  outline: none;
  border-color: #007bff;
}

select:focus,
button:focus {
  box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
}

select:disabled {
  background-color: #e9ecef;
}
</style>
