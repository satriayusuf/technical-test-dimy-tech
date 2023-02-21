<template>
    <div class="p-10">
        <button @click="addRow" class="px-3 py-1 rounded bg-gray-400"> New </button>
        <div v-for="(row, index) in rows" :key="index + 1" class="grid grid-cols-12 gap-4 items-end mt-6">
            <div class="col-span-3">
                <span> Product Name </span>
                <input type="text" v-model="rows[index].productName" class="w-full border border-gray rounded pl-1 py-2 mt-1" :placeholder="`Product ${index + 1}`">
            </div>
            <div class="col-span-3">
                <span> Product Price </span>
                <input type="number" v-model="rows[index].productPrice" class="w-full border border-gray rounded pl-1 py-2 mt-1" @input="setTotal(index)">
            </div>
            <div class="col-span-3">
                <span> Quantity </span>
                <input type="number" v-model="rows[index].quantity" class="w-full border border-gray rounded pl-1 py-2 mt-1" @input="setTotal(index)">
            </div>
            <div class="col-span-2">
                <span> Total </span>
                <div class="flex gap-3 items-center">
                    <input type="number" v-model="rows[index].total" disabled class="w-full border border-gray rounded pl-1 py-2 mt-1">
                </div>
            </div>
            <div class="col-span-1">
                <button v-if="index > 0" @click="deleteRow(index)" class="px-3 py-1 rounded bg-red-400 text-white"> Delete </button>
            </div>
        </div>
    
        <div class="grid grid-cols-1 justify-end gap-4 mt-10">
            <div class="col-span-1 flex justify-end">
                <div>
                    <span> Grand Total </span>
                    <input type="number" v-model="grandTotal" class="w-full border border-gray rounded pl-1 py-2 mt-1">
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const grandTotal = ref(0)

const rows = ref([{
    productName: '',
    productPrice: 0,
    quantity: 0,
    total: 0
}])


function addRow() {
    const formRow = {
        productName: '',
        productPrice: 0,
        quantity: 1,
        total: 0
    }

    rows.value.push(formRow)
}

function deleteRow(indexRows) {
    rows.value.splice(indexRows, 1)
    setGrandTotal()
}

function setTotal(indexRows) {
    setAlertQuantity(indexRows)

    rows.value[indexRows].total = rows.value[indexRows].productPrice * rows.value[indexRows].quantity

    setGrandTotal()
}

async function setAlertQuantity(indexRows) {
    if (rows.value[indexRows].quantity < 1) {
        await alert('quantity tidak boleh kurang dari satu')
        rows.value[indexRows].quantity = 1
    }
}

async function setGrandTotal() {
    let summaryGrandTotal = []

    for (let i = 0; i < rows.value.length; i++) {
        summaryGrandTotal.push(rows.value[i].total)
    }

    grandTotal.value = summaryGrandTotal.reduce((acc, curr) => acc + curr)

}
</script>

