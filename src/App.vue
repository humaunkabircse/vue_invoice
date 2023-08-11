<script setup>
import {reactive} from 'vue'

const data = reactive({
  sender: '',
  billTo: '',
  shipTo: '',
  invoiceNumber: '',
  invoiceDate: '',
  dueDate: '',
  additionalNotes: '',
  items: [{description: '', quantity: '', rate: '', discount: '', amount: ''}],
  notes: '',
  trems: '',
  subTotal: '',
  tax: '',
  total: ''
})

// Delete Items
function deleteItem(index){
  if (index === 0) {
    return;
  }
  data.items.splice(index, 1)
}

function getSubTotal() {
  let subTotal = 0
  data.items.forEach((item) => {
    subTotal += item.amount
  })
  data.subTotal = subTotal
  return subTotal
}

// Get getTotal() function
function getTotal(){
  const tax = data.subTotal * data.tax / 100
  data.total = data.subTotal + tax
  return data.total 
}

// get addMoreItem() function
  function getAddMoreItem(){
    data.items.push({description: '', quantity: '', rate: '', discount: '', amount: ''})
  }

// Get print() function
function getPrint(){
    window.print()
}
</script>

<template>
  <section class="mx-auto container bg-white border border-gray-400 min-h-screen p-12" style="background: #13071f; color: #811111;">
        <div class="flex justify-between">
            <div class="flex flex-col space-y-5 w-1/2s">
                <div class=" ">
                    <img class="w-40" src="https://www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg" alt="">
                </div>
                <p class="mt-5">
                    Sender
                </p>
                <textarea name="" id="" cols="30" rows="2" v-model="data.sender"></textarea>
                <div class="flex space-x-2">
                    <div class="flex flex-col">
                        <span>Bill to</span>
                        <textarea name="" id="" cols="30" rows="2" v-model="data.billTo"></textarea>
                    </div>
                    <div class="flex flex-col">
                        <span>Ship to</span>
                        <textarea name="" id="" cols="30" rows="2" v-model="data.shipTo"></textarea>
                    </div>
                </div>
            </div>
            <div class="flex flex-col w-1/2 items-end">
                <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
                <input class="w-[200px] text-right" type="text" placeholder="Invoice Number" v-model="data.invoiceNumber">
                <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
                    <p>
                        <span>Date</span>
                        <input  class="ml-2 w-[200px] " v-model="data.invoiceDate">
                    </p>
                    <p>
                        <span>Due Date</span>
                        <input  class="ml-2 w-[200px]" v-model="data.dueDate">
                    </p>
                    <p>
                        <span>Additional Note</span>
                        <input class="ml-2 w-[200px]" type="text" v-model="data.additionalNotes">
                    </p>
                </div>
            </div>
        </div>
        {{ data }}
        <div class="mt-20">
            <table class="table-auto w-full">
                <tr class="bg-gray-800 text-left text-white">
                    <th class="p-2 pl-5 w-1/2">Item</th>
                    <th class="p-2">Quantity</th>
                    <th class="p-2">Rate</th>
                    <th class="p-2">Discount</th>
                    <th class="p-2 w-[200px] text-right pr-5">Amount</th>
                </tr>
                <tr v-for="(item, index) in data.items" :key="index">
                    
                    <td class="py-1">
                        <input class="w-full pl-5" type="text" placeholder="Description" v-model="item.description"/>
                    </td>
                    <td class="">
                        <input class="w-[100px]" type="number" placeholder="Quantity" v-model="item.quantity"/>
                    </td>
                    <td class="">
                        <input class="w-[50px]" type="number" placeholder="Rate" v-model="item.rate">
                    </td>
                    <td class="">
                        <input class="w-[100px]" type="number" placeholder="Discount" v-model="item.discount">
                    </td>
                    <td class="py-1 pr-5 text-right text-gray-800">
                        $ {{ item.amount = (item.quantity * item.rate) - item.discount }}
                    </td>
                    <td class="text-center w-[20px]">
                      <button @click="deleteItem(index)" class="btn bg-red-500 px-2 py-0.5 rounded
                      text-white">X</button>
                    </td>
                </tr>
            </table>
            <button @click="getAddMoreItem()" class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
                Add More
            </button>
            <button @click="getPrint()" class="ml-2 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
                Print
            </button>
        </div>
        <div class="mt-[200px]">
            <div class="flex justify-between">
                <div class="flex flex-col space-y-5 w-1/2">
                    <span>Notes</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.notes"></textarea>
                    <span>Terms</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.trems"></textarea>
                </div>
                <div class="flex flex-col w-1/2 items-end">
                    <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
                        <p>
                            <span>Subtotal</span>
                            <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Subtotal" :value="getSubTotal()">
                        </p>
                        <p>
                            <span>Tax</span>
                            <input type="number" class="tax text-right w-[200px] ml-2" v-model="data.tax">
                        </p>
                        <p>
                            <span>Total</span>
                            <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Total" :value="getTotal()">
                        </p>
                        <p>
                            <span>Balace Due</span>
                            <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Balance">
                        </p>
                    </div>
                </div>
            </div>
        </div>

    </section>
</template>

<style scoped></style>
