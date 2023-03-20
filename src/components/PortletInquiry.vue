<template>
    <div class="grid place-items-center">
        <div class="w-3/5 p-4 text-center bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700"> 
            <form>
                <div class="mb-6">
                    <select v-model="form.payment_type" id="payment-type" class="p-2 bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                        <option selected disabled>Pilih Jenis Pembayaran</option>
                        <option v-for="(result) in results" :key="result.code" :value="result.code">{{ result.code }} {{ result.description}}</option>
                    </select>
                </div>
                <div class="mb-6">
                    <div class="relative">
                        <input v-model="form.id_billing" type="text" id="floating_outlined" class="block px-2.5 pb-2.5 pt-4 w-full text-sm text-gray-900 bg-transparent rounded-lg border-1 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                        <label v-if="this.form.payment_type == 0" for="floating_outlined" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin-[0] bg-white dark:bg-gray-900 px-2 peer-focus:px-2 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 left-1">
                            Kode VA + ID Billing
                        </label>
                        <label v-else for="floating_outlined" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin-[0] bg-white dark:bg-gray-900 px-2 peer-focus:px-2 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 left-1">
                            ID Billing
                        </label>
                    </div>
                </div>
                <div class="mb-6">
                    <ul class="grid w-full gap-6 md:grid-cols-2">
                        <li>
                            <input v-model="form.payment_method" type="radio" id="virtual-account" name="payment-method" value="1" class="hidden peer" required>
                            <label for="virtual-account" class="inline-flex items-center justify-between w-full p-5 text-gray-500 bg-white border border-gray-200 rounded-lg cursor-pointer dark:hover:text-gray-300 dark:border-gray-700 dark:peer-checked:text-blue-500 peer-checked:border-blue-600 peer-checked:text-blue-600 hover:text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:bg-gray-800 dark:hover:bg-gray-700">                           
                                <div class="block">
                                    <div class="flex items-center mr-4">
                                        <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                                        <label for="inline-radio" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Virtual Account </label>
                                    </div>
                                </div>
                            </label>
                        </li>
                        <li>
                            <input v-model="form.payment_method" type="radio" id="qris" name="payment-method" value="2" class="hidden peer" required>
                            <label for="qris" class="inline-flex items-center justify-between w-full p-5 text-gray-500 bg-white border border-gray-200 rounded-lg cursor-pointer dark:hover:text-gray-300 dark:border-gray-700 dark:peer-checked:text-blue-500 peer-checked:border-blue-600 peer-checked:text-blue-600 hover:text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:bg-gray-800 dark:hover:bg-gray-700">                           
                                <div class="block">
                                    <div class="flex items-center mr-4">
                                        <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                                        <label for="inline-radio" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">QRIS</label>
                                    </div>
                                </div>
                            </label>
                        </li>
                    </ul>
                </div>
                <div class="mb-6">
                    <div class="w-full">
                        <button type="button" @click="generatePayment" class="w-full text-white bg-blue-700 hover:bg-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Proses Pembayaran</button>
                    </div>
                </div>
            </form>
            <hr class="border-dashed">
            <div v-show="this.form.payment_method == 1" class="p-2">
                <p>{{ paymentRes.virtual_account }}</p>
                <p>{{ paymentRes.customer_name }}</p>
                <p>{{ paymentRes.trx_amount }}</p>
                <p>{{ paymentRes.datetime_expired }}</p>
                <p>{{ paymentRes.payment_type }}</p>
                <p>{{ paymentRes.id_billing }}</p>
            </div>
            <div v-show="this.form.payment_method == 2" class="p-2">
                <div v-html="paymentRes.qris"></div>
                <p>{{ paymentRes.payment_type }}</p>
                <p>{{ paymentRes.id_billing }}</p>
            </div>
        </div>
    </div> 
</template>

<script>
    import axios from 'axios'
    export default {
        data() {
            return {
                form:{},
                results: [],
                paymentRes :[]
            }
        },
        async mounted() {
            try {
            await axios
                .get('http://10.16.4.3:8089/api/v1/payment/type')
                .then(response => this.results = response.data.data.payment_type)
                // console.log(this.results);
            } catch(err) {
            console.log(err)
            }
        },
        methods: {
            generatePayment() {
                const param = { 
                    id_billing:this.form.id_billing,
                    payment_type:this.form.payment_type,
                    payment_method:this.form.payment_method
                };
                // const param1 = {
                //     id_billing:"220301107174",
                //     payment_type:"4",
                //     payment_method:"1"
                // }
                console.log(param)
                axios.post("http://10.16.4.3:8089/api/v1/payment/generate", param)
                    .then(response => this.paymentRes = response.data.data)
                    .catch(error => {
                     
                    this.errorMessage = error.message;
                    console.error("There was an error!", error);
                });

                console.error(this.paymentRes.qris);
            }
        }
    }
</script>