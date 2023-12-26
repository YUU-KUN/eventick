<template>
    <div class="flex flex-col items-center justify-center h-screen w-screen" :class="main_bg">
        <div class="absolute top-12 left-16 flex items-center justify-start">
            <img src="/public/assets/img/main-logo-black.png" alt="main-logo-black">
        </div>
        <img v-if="!result" src="/public/assets/img/ticket-bg.png" alt="ticket-bg" class="absolute w-full h-full">
        <img class="absolute z-10 h-full"
            :src="result && result.agency == 'YG' ? '/public/assets/img/YG.png' : '/public/assets/img/SM.png'" alt="">

        <div v-if="result == null"
            class="text-center bg-primary py-16 px-14 w-2/5 rounded-3xl text-white z-20 drop-shadow-2xl">
            <div class="flex flex-col items-start justify-center w-full mb-4">
                <div class="w-full text-left">
                    <label for="agency" class="text-24">Agency</label>
                    <select v-model="agency" name="agency" id="agency"
                        class="font-bold text-20 w-full bg-transparent border-b border-b-white mt-5 pb-5 focus:outline-none">
                        <option value="" selected disabled>Choose Agency</option>
                        <option value="SM">SMWTOWN</option>
                        <option value="YG">YG Entertainment</option>
                    </select>
                </div>
                <p class="w-full text-center text-24 my-10">OR</p>
                <div class="w-full text-left">
                    <label for="agency" class="text-24">Ticket ID</label>
                    <input v-model="ticket_id" type="text"
                        class="font-bold text-20 w-full bg-transparent border-b border-b-white mt-5 pb-5 focus:outline-none"
                        placeholder="Input Ticket ID">
                </div>
            </div>
            <button @click="search()"
                class="font-bold text-18 bg-pink rounded-full w-[230px] py-5 flex items-center justify-center mx-auto">
                <IconLoading v-if="is_searching" />
                <span v-else>Cari</span>
            </button>
        </div>

        <!-- w-2/3 drop-shadow-2xl-->
        <div v-else class="relative flex flex-col items-center justify-start w-4/5 rounded-3xl z-20" :class="card_bg">
            <div class="absolute -top-14 right-0 z-20 flex items-center justify-end w-full">
                <img src="/public/assets/img/download.png" alt="download" class="mr-3">
                <img @click="reset()" src="/public/assets/img/exit.png" alt="exit" class="cursor-pointer">
            </div>
            {{ getResultStyle(result.agency) }}

            <!-- Ticket Card -->
            <!-- w-[899px] my-10 h-[376px]-->
            <div class="relative flex flex-col items-center justify-center rounded-3xl font-normal text-40 p-10 w-full px-24"
                :class="[round_bg, getTextColor(result.agency)]">
                <div class="flex flex-col items-center justify-center absolute -right-1 top-1">
                    <div v-for="i in 10" :key="i" class="rounded-full z-20 h-10 w-10 mb-3" :class="main_bg"></div>
                </div>
                <div class="grid grid-cols-6 w-full h-full">
                    <div class="col-span-4 mr-28">
                        <div
                            class="relative flex flex-col items-start justify-start rounded-[70px] border-4 border-black py-10 px-14">
                            <img src="/public/assets/img/SM.png" alt="SM" class="w-[50px] h-[50px] absolute right-14 top-5">
                            <div class="w-4/5 border-b border-black mb-10 py-3">
                                <p class="font-normal text-51">{{ getAgency(result.agency) }}</p>
                            </div>
                            <div class="w-4/5 border-b border-black mb-12 py-3">
                                <p class="font-normal text-51">{{ result.event_name }}</p>
                            </div>

                            <div class="flex items-stretch justify-between w-full h-full">
                                <div class="flex flex-col items-start justify-between h-full">
                                    <p class="text-30 font-normal mb-1">Date:</p>
                                    <div class="flex justify-center items-center">
                                        <div class="text-center">
                                            <p class="text-14 font-normal ">Year</p>
                                            <p class="font-extrabold text-20">{{ year }}</p>
                                        </div>
                                        <div class="h-14 border-l border-black mx-2"></div>
                                        <div class="text-center">
                                            <p class="text-14 font-normal">Month</p>
                                            <p class="font-extrabold text-20">{{ month }}</p>
                                        </div>
                                        <div class="h-14 border-l border-black mx-2"></div>
                                        <div class="text-center">
                                            <p class="text-14 font-normal">Date</p>
                                            <p class="font-extrabold text-20">{{ date }}</p>
                                        </div>
                                        <div class="h-14 border-l border-black mx-2"></div>
                                        <div class="text-center">
                                            <p class="text-14 font-normal">Day</p>
                                            <p class="font-extrabold text-20">{{ day }}</p>
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-col items-start justify-between h-full">
                                    <p class="text-30 font-normal mb-6">Time:</p>
                                    <p class="font-extrabold text-24 ">{{ time }}</p>
                                </div>
                                <div class="flex flex-col items-start justify-between h-full">
                                    <p class="font-normal text-30 mb-6">Address:</p>
                                    <p class="font-extrabold text-24 ">{{ result.place }}, {{ result.city }}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-span-2">
                        <div class="-rotate-90 h-full w-full px-4">
                            <div
                                class="flex flex-col items-center justify-center rounded-[70px] border-4 border-black py-6 px-11">
                                <div class="flex items-center justify-between w-full mb-5">
                                    <div>
                                        <p class="text-14 font-normal">Gate</p>
                                        <p class="font-extrabold text-36">01</p>
                                    </div>
                                    <div class="h-14 border-l border-black"></div>
                                    <div>
                                        <p class="text-14 font-normal">Row</p>
                                        <p class="font-extrabold text-36">02</p>
                                    </div>
                                    <div class="h-14 border-l border-black"></div>
                                    <div>
                                        <p class="text-14 font-normal">Seat</p>
                                        <p class="font-extrabold text-36">03</p>
                                    </div>
                                </div>
                                <div>
                                    <vue-barcode class="w-full mb-5" value="Hello, World!" :options="options"></vue-barcode>
                                    <div class="flex items-center justify-between w-full text-18 font-normal">
                                        <p>Ticket ID :</p>
                                        <p>{{ result.tiket_id }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="flex flex-col items-center justify-center absolute -left-1 top-1">
                    <div v-for="i in 10" :key="i" class="rounded-full z-20 h-10 w-10 mb-3" :class="main_bg"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import IconLoading from '@/components/icons/IconLoading.vue'
import moment from 'moment-timezone'

export default {
    components: { IconLoading, VueBarcode },
    data() {
        return {
            is_searching: false,
            agency: '',
            ticket_id: '',
            // result: {
            //     agency: 'YG',
            // },
            result: null,

            main_bg: 'bg-white',
            card_bg: 'bg-primary',
            round_bg: '',
            options: { displayValue: false, background: 'none' },
            year: '',
            month: '',
            date: '',
            day: '',
            time: '',
        }
    },
    methods: {
        search() {
            this.is_searching = true
            this.axios.get(`event/search?ticket_id=${this.ticket_id}&agency=${this.agency}`).then(response => {
                this.result = response.data
                this.getDate(this.result.date)
            }).catch(error => {
                console.log(error);
            }).finally(() => {
                this.is_searching = false
            })
        },
        getResultStyle(agency) {
            switch (agency) {
                case "SM":
                    this.main_bg = 'bg-sm-bg'
                    this.card_bg = 'bg-sm-card'
                    this.round_bg = 'bg-sm-round'
                    this.options.background = '#FFD0D9'
                    this.options.lineColor = '#000000'
                    break;
                case "YG":
                    this.main_bg = 'bg-yg-bg'
                    this.card_bg = 'bg-yg-card'
                    this.round_bg = 'bg-yg-round'
                    this.options.background = '#000000'
                    this.options.lineColor = '#FFFFFF'
                    break;
                default:
                    this.main_bg = 'bg-white'
                    this.card_bg = 'bg-primary'
                    this.round_bg = ''
                    break;
            }
        },
        getAgency(agency) {
            return agency == 'SM' ? 'SMTOWN' : 'YG Entertainment'
        },
        getDate(date) {
            // 2023-10-10T19:00:00.000Z
            this.year = date.substring(0, 4)
            //Jan
            this.month = moment(date.substring(5, 7), 'MM').format('MMM')
            this.date = date.substring(8, 10)
            // Thu
            this.day = moment.tz(date, 'Asia/Jakarta').format('dddd')
            // 4:30 PM
            this.time = moment.tz(date, 'Asia/Jakarta').format('hh:mm A')
        },
        getTextColor(agency) {
            return agency === 'SM' ? 'text-black' : 'text-black'
        },
        reset() {
            this.result = null
            this.ticket_id = ''
            this.agency = ''
        }
    },
}
</script>

<style></style>