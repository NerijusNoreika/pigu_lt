<template>

    <tr @click="select(); selectedRow = !selectedRow" :class="{' bg-gray-100' : selectedRow}" class="border-b border-gray-200 hover:bg-gray-100">
        <td class="py-3 px-6 text-center whitespace-nowrap">{{ id }}</td>
        <td class="py-3 px-6 text-center whitespace-nowrap">{{ name }}</td>
        <td class="py-3 px-6 text-center whitespace-nowrap">{{ surname }} </td>
        <td class="py-3 px-6 text-center whitespace-nowrap">{{ department }}</td>
        <td class="py-3 px-6 text-center whitespace-nowrap">{{ salary }} €</td>
    </tr>
</template>

<script>
export default {
    name: 'TableRow',
    props: ['id', 'department', 'name', 'surname', 'salary', 'selected'],
    beforeMount() {
        
    },
    data() {
        return {
            selectedRow: this.selected,
        }
    },
    methods: {
        select() {
            this.$emit('showOutside', this.id);
        }
    }
}
</script>

<style>

</style>