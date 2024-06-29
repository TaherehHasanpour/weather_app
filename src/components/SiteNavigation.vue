<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav
        class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >


      <div class="flex gap-3 flex-1 justify-start">
        <i
            class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
            @click="toggleModal"
        ></i>
        <i
            class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
            @click="addCity"
            v-if="route.query"
        ></i>
      </div>

      <BaseModal
          :modalActive="modalActive"
          @close-modal="toggleModal"
      >
        <div class="text-black">
          <h1 class="text-2xl mb-1 text-right font-IranYekan"> :درباره</h1>
          <p class="mb-4 text-right">
            .آب و هوای محلی به شما امکان می دهد آب و هوای فعلی و آینده شهرهای مورد نظر خود را ردیابی کنید
          </p>
          <h2 class="text-2xl text-right">:چگونه کار می کند</h2>
          <ol class=" list-inside mb-4 text-right">
            <li>
              .با وارد کردن نام در نوار جستجو، شهر خود را جستجو کنید
            </li>
            <li>
              .یک شهر را در نتایج انتخاب کنید، این شما را به آب و هوای فعلی برای انتخاب خود می برد
            </li>
            <li>
              با کلیک بر روی نماد "+" در بالا سمت راست، شهر را ردیابی کنید. با این کار شهر ذخیره می شود تا در زمان دیگری
              در صفحه اصلی مشاهده شود
            </li>
          </ol>

          <h2 class="text-2xl text-right">حذف یک شهر</h2>
          <p class="text-right">
            اگر دیگر نمی‌خواهید شهری را ردیابی کنید، به سادگی شهر را در صفحه اصلی انتخاب کنید. در پایین صفحه گزینه ای
            برای حذف شهر وجود دارد
          </p>
        </div>
      </BaseModal>
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">آب و هوا</p>
        </div>
      </RouterLink>
    </nav>
  </header>
</template>

<script setup>
import {RouterLink, useRoute, useRouter} from "vue-router";
import {uid} from "uid";
import {ref} from "vue";
import BaseModal from "./BaseModal.vue";

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
        localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem(
      "savedCities",
      JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({query});
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
