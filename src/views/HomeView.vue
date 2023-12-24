<template>
  <div class="w-screen h-full">
    <header class="z-20 absolute top-0 left-0 flex items-center justify-between w-full px-28 py-7 text-white">
      <img src="/public/assets/img/main-logo.png" alt="main-logo">
      <div class="flex items-center justify-end w-full">
        <a class="mr-10" href="Schedule">Schedule</a>
        <a class="mr-10" href="Speakers">Speakers</a>
        <a class="mr-10" href="Ticket">Ticket</a>
        <a class="mr-12" href="Contact">Contact</a>
        <button class="rounded-full px-7 py-2 border border-white font-medium text-16">Login</button>
      </div>
    </header>

    <!-- Banner -->
    <!-- h-[720px] -->
    <div class="relative flex flex-col items-center justify-center w-full h-screen text-white">
      <img src="/public/assets/img/banner.png" alt="banner" class="absolute top-0 left-0 -z-10 w-full h-full">
      <div class="flex items-center justify-center w-full">
        <img src="/public/assets/img/boyband.png" alt="boyband" class="z-20">
        <div class="w-[443px]">
          <p class="font-bold text-40 mb-6">SBS MTV The Kpop Show Ticket  Package</p>
          <p class="font-normal text-18 mb-8">Look no further! Our SBS The Show tickets are the simplest way for you to experience a live Kpop recording.</p>
          <div class="flex items-center justify-start w-full">
            <button @click="goToSearchTicket" class="rounded-full bg-pink font-bold text-18 py-4 w-[182px] mr-5">Search Ticket</button>
            <button class="rounded-full bg-transparent border border-white font-bold text-18 py-4 w-[182px] mr-5">Learn More</button>
          </div>
        </div>
      </div>
      <!-- <div class="absolute -bottom-12 bg-primary px-12 py-9 w-[1086px] rounded-3xl ">
        <div class="grid grid-cols-3 w-full gap-14">
          <div>
            <label for="event">Search Event</label>
            <input id="event" type="text" class="border-b border-white focus:outline-none w-full pb-3 font-bold text-20 bg-transparent mt-3">
          </div>
          <div>
            <label for="place">Place</label>
            <input id="place" type="text" class="border-b border-white focus:outline-none w-full pb-3 font-bold text-20 bg-transparent mt-3">
          </div>
          <div>
            <label for="time">Time</label>
            <input id="time" type="text" class="border-b border-white focus:outline-none w-full pb-3 font-bold text-20 bg-transparent mt-3">
          </div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    goToSearchTicket() {
      this.$router.push({
        name: 'search-ticket'
      })
    }
  }
}
</script>

<style>

</style>