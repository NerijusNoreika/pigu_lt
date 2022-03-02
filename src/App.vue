<template>
    <div>
        <div class="min-w-screen min-h-screen bg-gray-100 flex justify-center bg-gray-100 font-sans overflow-hidden">
                <div class="sm:p-5 md:p-5 w-full lg:w-4/5">
                <div class="overflow-x-auto bg-white shadow-md rounded my-6">
                    <DataTable />
                </div>
        </div>
      </div>
    </div>
</template>

<script>
  import DataTable from './components/DataTable.vue';
export default {
  name: 'App',
  components: {
    DataTable
  }
}
</script>

<style>

</style>
