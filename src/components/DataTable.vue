<template>
  <div
    v-if="outside"
    :class="{ 'py-3': outside }"
    class="justify-around flex"
  >
    <span>{{ this.workers[this.outsideIndex]?.id }}</span>
    <span>{{ this.workers[this.outsideIndex]?.name }}</span>
    <span>{{ this.workers[this.outsideIndex]?.name }}</span>
    <span>{{ this.workers[this.outsideIndex]?.surname }}</span>
    <span>{{ this.workers[this.outsideIndex]?.salary }}</span>
    <span>{{ this.workers[this.outsideIndex]?.department }}</span>
  </div>
  <table class="sm:p-5 w-full table-auto">
    <thead>
      <tr
        class="
          sm:text-left
          md:text-center
          sm:table-row
          bg-gray-200
          text-gray-600
          uppercase
          text-lg
          leading-normal
        "
      >
        <th @click="sort('id')" class="py-3 px-3 cursor-pointer">#<i class="ml-1 text-md fas fa-sort"></i></th>
        <th @click="sort('name')" class="py-3 px-3 cursor-pointer">
          Vardas<i class="ml-1 text-md fas fa-sort"></i>
        </th>
        <th @click="sort('surname')" class="py-3 px-3 cursor-pointer">
          Pavardė<i class="ml-1 text-md fas fa-sort"></i>
        </th>
        <th @click="sort('department')" class="py-3 px-3 cursor-pointer">
          Padalinys<i class="ml-1 text-md fas fa-sort"></i>
        </th>
        <th @click="sort('salary')" class="py-3 px-3 cursor-pointer">
          Mėn. atlyginimas<i class="ml-1 text-md fas fa-sort"></i>
        </th>
      </tr>
    </thead>
    <tbody class="text-gray-600 text-md font-light">
      <TableRow
        @click="getContent(incident.id)"
        v-on:showOutside="showOutside"
        v-for="worker in workers"
        :municipality="worker.municipality_name"
        :name="worker.name"
        :surname="worker.surname"
        :salary="worker.salary"
        :department="worker.department"
        :key="worker.id"
        :id="worker.id"
        :selected="false"
      />
    </tbody>
  </table>
  <div class="flex gap-2 justify-center">
    <button
      class="
        block
        bg-gray-300
        hover:bg-gray-400
        text-gray-800
        font-bold
        my-5
        py-2
        px-4
        rounded
      "
      @click="loadMore"
    >
      Užkrauti papildomus duomenis
    </button>
    <button
      class="
        block
        bg-gray-300
        hover:bg-gray-400
        text-gray-800
        font-bold
        my-5
        py-2
        px-4
        rounded
      "
      @click="loadAll"
    >
      Užkrauti visus duomenis
    </button>
  </div>
</template>

<script>
import axios from "axios";
import TableRow from "./TableRow.vue";
export default {
  name: "DataTable",
  data() {
    return {
      outsideIndex: "",
      outside: false,
      sortOrder: true,
      allShowing: false,
      URL_LOAD_MORE: "https://pigu.goodpixel.eu/api/workers",
      URL_LOAD_ALL: "https://pigu.goodpixel.eu/api/workers/all",
      workers: [],
      meta: [],
      page: 1,
    };
  },
  methods: {
    getContent(id) {
      this.columns = this.incidents.filter((row) => row.id === id);
    },
    loadAll() {
      axios.get(this.URL_LOAD_ALL).then((content) => {
        this.workers = [];
        this.allShowing = true;
        this.page = 1;

        content.data.forEach((worker) => {
          this.workers.push(worker);
        });
        this.meta = content.data.meta;
      });
    },
    sort: function (method) {
        this.outside = false;
        this.outsideIndex = '';
    let order = this.sortOrder;
        if(method == 'id') {
              if (this.sortOrder) {
              this.workers.sort((a, b) => b.id - a.id);
          } else {
              this.workers.sort((a, b) => a.id - b.id);
          }
          this.sortOrder = !this.sortOrder;
        }
      if (method == "name") {
        this.workers.sort(function (a, b) {
          var nameA = a.name.toUpperCase(); 
          var nameB = b.name.toUpperCase(); 
          if (nameA < nameB) {
            return order ? -1 : 1;
          }
          if (nameA > nameB) {
           return order ? 1 : -1;
          }
          return 0;
        });
        this.flipOrder();
      }

      if (method == "surname") {
        let order = this.sortOrder;
        this.workers.sort(function (a, b) {
          var nameA = a.surname.toUpperCase(); 
          var nameB = b.surname.toUpperCase(); 
          if (nameA < nameB) {
           return order ? -1 : 1;
          }
          if (nameA > nameB) {
            return order ? 1 : -1;
          }
          return 0;
        });
        this.flipOrder();
      }

      if (method == "department") {
        let order = this.sortOrder;
        console.log(order);
        this.workers.sort(function (a, b) {
          var nameA = a.department.toUpperCase(); 
          var nameB = b.department.toUpperCase(); 
          if (nameA < nameB) {
            return order ? -1 : 1;
          }
          if (nameA > nameB) {
            return order ? 1 : -1;
          }
          return 0;
        });
        this.flipOrder();
      }

      if (method == "salary") {
          if (this.sortOrder) {
              this.workers.sort((a, b) => a.salary - b.salary);
          } else {
              this.workers.sort((a, b) => b.salary - a.salary);
          }
          this.sortOrder = !this.sortOrder;
      }
    },
    loadMore() {
      axios
        .get(this.URL_LOAD_MORE, { params: { page: this.page } })
        .then((content) => {
          if (this.allShowing) {
            this.workers = [];
            this.allShowing = false;
            this.page = 1;
          }
          content.data.data.forEach((worker) => {
            this.workers.push(worker);
          });
          this.meta = content.data.meta;
        });
      this.page++;
    },
    showOutside(id) {
      this.outside = true;
      this.outsideIndex = id;
    },
    flipOrder() { 
      this.sortOrder = !this.sortOrder;  
    },
  },
  components: {
    TableRow,
  },
  beforeMount() {
    this.loadMore();
  },
};
</script>

<style>
</style>