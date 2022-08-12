<script setup>
import { ref, onMounted, computed } from "vue";

// reactive state
const bookListRaw = ref([]);
const bookList = ref([]);
const filterStart = ref("");
const filterEnd = ref("");
const filteringFlag = ref(false);

const enabledFilter = computed(() => filterStart.value && filterEnd.value);

function applyFilter() {
  if (filterStart.value && filterEnd.value) {
    let sd = new Date(filterStart.value).getTime();
    let ed = new Date(filterEnd.value).getTime();
    bookList.value = bookListRaw.value.filter((item) => {
      let bookDate = new Date(item.createAt).getTime();
      return sd < bookDate && bookDate < ed;
    });
    filteringFlag.value = true;
  }
}

function undoFilter() {
  filteringFlag.value = false;
  filterStart.value = "";
  filterEnd.value = "";
  bookList.value = bookListRaw.value;
}

onMounted(() => {
  const url = import.meta.env.VITE_APP_ROOT_API + "/books";
  fetch(url)
    .then((resp) => resp.json())
    .then((data) => {
      if (data.status == "success") {
        bookListRaw.value = data.data;
        bookList.value = data.data;
      }
    })
    .catch(function (error) {
      console.log(error);
    });

  function checkInput(type) {
    var input = document.createElement("input");
    input.setAttribute("type", type);
    return input.type == type;
  }

  if (!checkInput("date")) {
    jQuery(document).ready(function () {
      jQuery(".datepicker-start")
        .prop("readonly", true)
        .datepicker({
          onSelect: function (dateText) {
            filterStart.value = dateText;
          },
      });
      jQuery(".datepicker-end")
        .prop("readonly", true)
        .datepicker({
          onSelect: function (dateText) {
            filterEnd.value = dateText;
          },
      });
    });
  };
});
</script>

<template>
  <div class="libros">
    <h2>Catalogo de libros</h2>
    <form class="form-filter">
      <div class="row mb-5">
        <div class="col">
          <label for="">Filtrar del:</label>
          <input
            type="date"
            class="datepicker-start form-control"
            v-model="filterStart"
            placeholder="dd/mm/aaaa"
            :class="{ 'empty-input': !filterStart }"
          />
        </div>
        <div class="col">
          <label for="">Al:</label>
          <input
            type="date"
            class="datepicker-end form-control"
            v-model="filterEnd"
            placeholder="dd/mm/aaaa"
            :class="{ 'empty-input': !filterStart }"
          />
        </div>
        <div class="col">
          <button
            type="button"
            class="btn btn-success"
            @click="undoFilter"
            v-if="filteringFlag"
          >
            Deshacer
          </button>
          <button
            type="button"
            class="btn btn-success"
            :disabled="!enabledFilter"
            @click="applyFilter"
            v-else
          >
            Filtrar
          </button>
        </div>
      </div>
    </form>


    <div class="grid-wrapper" v-if="bookList.length">
      <div
        class="grid-item text-center"
        v-for="(item, ix) in bookList"
        :key="ix"
      >
        <div class="inner-box">
          <span class="minicover"><img :src="item.cover" alt="" /></span>
          <div class="info-book">
            <b>{{ item.title }}</b>
            <span>{{ item.author }}</span>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <em>No hay libros</em>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .libros {
    min-height: 100vh;
    align-items: center;
  }
  .form-filter {
    width: 510px;
    max-width: 100%;
  }
}
.form-filter .row {
  align-items: flex-end;
}
@media (max-width: 1023px) {
  .col {
    flex-basis: 100%;
    margin-bottom: 13px;
  }
  .col button {
    width: 100%;
  }
}
.minicover {
  display: block;
}
.minicover img {
  width: 100%;
}
.inner-box {
  width: 100%;
  margin: auto;
  box-shadow: 0px 0px 12px 4px rgba(0, 0, 0, 0.12);
  border-radius: 13px;
  overflow: hidden;
  padding-bottom: 6px;
}
.grid-wrapper {
  column-count: 5;
  column-gap: 37px;
}
.grid-item {
  margin: 0;
  display: grid;
  grid-template-rows: 1fr auto;
  margin-bottom: 36px;
  break-inside: avoid;
  width: 100%;
}
@media (max-width: 1140px) {
  .grid-wrapper {
    column-count: 4;
    column-gap: 28px;
  }
  .grid-item {
    margin-bottom: 24px;
  }
}
@media (max-width: 825px) {
  .grid-wrapper {
    column-count: 3;
    column-gap: 28px;
  }
  .grid-item {
    margin-bottom: 24px;
  }
}
@media (max-width: 643px) {
  .grid-wrapper {
    column-count: 2;
    column-gap: 24px;
  }
}
.info-book {
  padding: 7px 12px 3px;
}
.info-book b {
  display: block;
}
.empty-input {
  color:gray;
}
</style>
