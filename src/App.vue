<script setup>
  import { ref,reactive,computed } from 'vue'

  const data = reactive({
    sender  : '',
    billTo  : '',
    shipTo  : '',
    invoiceNumber  : '',
    date    : '',
    dueDate : '',
    additionalNOte : '',
    items : [
      {
        description : '',
        quantity : '',
        rate : '',
        discount : '',
        ammount : '',
      }
    ],
    notes : '',
    terms : '',
    subTotal : '',
    tax : '',
    total : '',
    payment : '',
    due : '',
  });


  function addItem(){
      data.items.push({
          description : '',
          quantity : '',
          rate : '',
          discount : '',
          ammount : '',
      })
  }

  function calculateSubTotal(){
    let subtotal = 0;

    data.items.forEach(item => {
      subtotal += item.ammount
    })

    data.subTotal = subtotal
    return subtotal
  }


  const calculateTotal = computed(()=>{
    const total = data.subTotal + (data.subTotal * data.tax)/100 
    data.total = total;
    return total;
  })

  const calculateDuePayment = computed(()=>{
    const due = data.total - data.payment
    data.due = due
    return due
  });

  function removeItem(index){
    data.items.splice(index,1);
  }

</script>

<template>
    <section class="mx-auto container bg-white border border-gray-400 min-h-screen p-12">
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
                        <input  class="ml-2 w-[200px] " v-model="data.date">
                    </p>
                    <p>
                        <span>Due Date</span>
                        <input  class="ml-2 w-[200px]" v-model="data.dueDate">
                    </p>
                    <p>
                        <span>Additional Note</span>
                        <input class="ml-2 w-[200px]" type="text" v-model="data.additionalNOte">
                    </p>
                </div>
            </div>
        </div>
        <div class="mt-20">
            <table class="table-auto w-full">
                <tr class="bg-gray-800 text-left text-white">
                  <th class="p-2 pl-5 ">#</th>
                    <th class="p-2 pl-5 w-1/2">Item</th>
                    <th class="p-2">Quantity</th>
                    <th class="p-2">Rate</th>
                    <th class="p-2">Discount</th>
                    <th class="p-2 w-[200px] text-right pr-5">Amount</th>
                </tr>
                <tr v-for="(item, index) in data.items" :key="index">
                    <td class="py-1">
                        <button @click="removeItem(index)" class="bg-red-500 hover:bg-red-700 text-white font-bold px-1 ml-3 " type="button"><i class="fa-solid fa-square-minus "></i></button>
                    </td>
                    <td class="py-1">
                        <input class="w-full pl-5" type="text" placeholder="Description" v-model="item.description"/>
                    </td>
                    <td class="">
                        <input class="w-full" type="number" placeholder="Quantity" v-model="item.quantity"/>
                    </td>
                    <td class="">
                        <input class="w-full" type="number" placeholder="Rate" v-model="item.rate">
                    </td>
                    <td class="">
                        <input class="w-full" type="number" placeholder="Discount" v-model="item.discount">
                    </td>
                    <td class="py-1 pr-5 text-right text-gray-800">
                       $ {{item.ammount = (item.quantity * item.rate) - item.discount }}
                    </td>
                </tr>
            </table>
            <button class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="addItem()">
                Add More
            </button>
        </div>

        <div class="mt-[200px]">
            <div class="flex justify-between">
                <div class="flex flex-col space-y-5 w-1/2">
                    <span>Notes</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.notes"></textarea>
                    <span>Terms</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.terms"></textarea>
                </div>
                <div class="flex flex-col w-1/2 items-end">
                    <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
                        <p>
                            <span>Subtotal</span>
                            <input readonly :value="calculateSubTotal()" class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Subtotal">
                        </p>
                        <p>
                            <span>Tax</span>
                            <input type="number" class="tax text-right w-[200px] ml-2" v-model="data.tax">
                        </p>
                        <p>
                            <span>Total</span>
                            <input readonly :value="calculateTotal" class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Total">
                        </p>
                        <p>
                            <span>Payment</span>
                            <input  v-model="data.payment" class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Payment">
                        </p>
                        <p>
                            <span>Balance Due</span>
                            <input readonly :value="calculateDuePayment" class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Balance">
                        </p>
                    </div>
                </div>
            </div>
        </div>

    </section>
</template>

<style scoped>
  input,
        textarea {
            border: 1px solid #ddd !important;
            padding: 5px;
            border-radius: 5px;
        }

        input.tax::-webkit-outer-spin-button,
        input.tax::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
</style>
