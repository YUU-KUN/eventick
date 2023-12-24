<template>
    <div class="flex items-center justify-center h-screen w-screen" :class="main_bg">
        <img v-if="!result" src="/public/assets/img/ticket-bg.png" alt="ticket-bg" class="absolute w-full h-full">
        <div v-if="result === null"
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
            <button @click="search()" class="font-bold text-18 bg-pink rounded-full w-[230px] py-5 flex items-center justify-center mx-auto">
                <IconLoading v-if="is_searching"/>
                <span v-else>Cari</span>
            </button>
        </div>
        <div v-else class="flex flex-col items-center justify-start w-2/3 rounded-3xl z-20 drop-shadow-2xl"
            :class="card_bg">
            {{ getResultStyle(result.agency) }}
            <div class="flex items-center justify-between w-full mb-5 py-4 px-10">
                <img src="/public/assets/img/sm-entertainment.png" alt="sm-entertainment">
                <p class="font-normal text-40">{{ result.agency }} Entertainment</p>
                <img @click="reset()"
                    :src="result.agency === 'SM' ? '/public/assets/img/sm-close.png' : '/public/assets/img/yg-close.png'"
                    alt="sm-close">
            </div>
            <hr class="w-full border-2 border-black">
            <div class="flex flex-col items-center justify-center rounded-3xl font-normal text-40 p-10 w-[899px] h-[376px] my-10"
                :class="[round_bg, getTextColor(result.agency)]">
                <p class="mb-12">{{ result.event_name }}</p>
                <p class="mb-12">{{ getDate(result.date) }}</p>
                <p class="mb-12">{{ result.place }}, {{ result.city }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import IconLoading from '@/components/icons/IconLoading.vue'
export default {
    components: {IconLoading},
    data() {
        return {
            is_searching: false,
            agency: '',
            ticket_id: '',
            result: null,

            main_bg: 'bg-white',
            card_bg: 'bg-primary',
            round_bg: '',
        }
    },
    methods: {
        search() {
            this.is_searching = true
            this.axios.get(`event/search?ticket_id=${this.ticket_id}&agency=${this.agency}`).then(response => {
                console.log(response);
                this.result = response.data
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
                    break;
                case "YG":
                    this.main_bg = 'bg-yg-bg'
                    this.card_bg = 'bg-yg-card'
                    this.round_bg = 'bg-yg-round'
                    break;
                default:
                    this.main_bg = 'bg-white'
                    this.card_bg = 'bg-primary'
                    this.round_bg = ''
                    break;
            }
        },
        getDate(date) {
            // 2023-10-10T19:00:00.000Z to 2023-10-10 Monday 07.00 PM
            // const d = new Date(date)
            // const day = ['Minggu', 'Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu']
            // return `${day[d.getDay()]} ${d.getHours()}.${d.getMinutes()}`
            return new Date(date).toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: 'numeric' })
        },
        getTextColor(agency) {
            return agency === 'SM' ? 'text-black' : 'text-white'
        },
        reset() {
            this.result = null
            this.ticket_id = ''
        }
    },
    mounted() {
        console.log(this.main_bg);
        console.log(this.card_bg);
        console.log(this.round_bg);
    },
    watch: {

    }
}
</script>

<style></style>