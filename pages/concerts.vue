<template>
  <div class="grid  min-h-screen bg-gradient-to-t from-blue-200 to-indigo-900 p-5 rounded-xl">
    <div>
      <h1 class="text-4xl sm:text-5xl md:text-7xl font-bold text-gray-200 mb-5">Concerts</h1>
        <div class="mt-10">
            <button  v-if="$auth.loggedIn" class="focus:outline-none inline-flex items-center justify-center w-10 h-10 mr-2 text-indigo-100 transition-colors duration-150 bg-indigo-700 rounded-lg hover:bg-indigo-800">
            <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <div class="flex text-white">
            <div class="p-2 w-8 flex-shrink-0"></div>
            <div class="p-2 w-8 flex-shrink-0"></div>
            <div class="p-2 w-full">Lieu</div>
            <div class="p-2 w-full">Date</div>
            <div  v-if="$auth.loggedIn" class="p-2 w-12 flex-shrink-0 text-right">Action</div>
            </div>
            
            <div v-for="concert in concerts" :key="concert.id" class="flex border-b border-gray-800 hover:bg-gray-800 hover:text-white">
                <div class="p-3 w-8 flex-shrink-0">⏱</div>
                <div class="p-3 w-full">{{concert.name }}</div>
                <div class="p-3 w-full">{{concert.date }}</div>
                <div  v-if="$auth.loggedIn" class="p-3 w-12">
                    <button @click="showConcert(concert)" class="focus:outline-none inline-flex items-center justify-center w-10 h-10 mr-2 text-gray-700 transition-colors duration-150 bg-white rounded-full hover:bg-gray-200">
                    <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"></path></svg>
                    </button>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import concertModal from '@/components/modals/concert.vue'
export default {
  layout: 'default',
  components: {
    concertModal
  },
  data() {
    return {
      concerts: [],
    }
  },
  created() {
    this.getConcerts()
  },
  methods: {
    getConcerts() {
      this.$axios.$get('/concerts')
      .then(response => {
        this.concerts = response
      })
      .catch( error => {
        console.log(error)
      })
    },

    showConcert (concert) {
      this.$modal.show(
          concertModal,
          {concert},
          {
            width: 500,
            height: 340
          },
          { draggable: true },
          // eslint-disable-next-line no-unused-vars
          { 'before-close': event => { this.getConcerts() } }
      )
    },

   }
}
</script>