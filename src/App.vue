<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container py-6">
      <div class="bg-white p-4 shadow rounded">
        <div class="flex -mx-4 items-center mb-6">
          <div class="flex-1 px-4">
            <select @change="getSpecificSurah" class="quran-input" name="" id="">
              <option value="">Select Surah</option>
              <option v-for="allSurah in allSurahs" :key="allSurah.number" :value="allSurah.number">{{allSurah.number}}. {{allSurah.name}} - {{allSurah.englishName}}</option>
            </select>
          </div>
          <div class="flex-1 px-4 text-center">
            <h3 class="font-bold mb-1 text-lg">{{ surahs.englishName}}</h3>
            <p>{{ surahs.englishNameTranslation}}</p>
          </div>
          <div class="flex-1 px-4 text-center">
          <h4>No. of Ayah: {{ surahs.numberOfAyahs }}</h4>
          <h4>{{ surahs.revelationType }}</h4>
          </div>
        </div>
        
        <div v-if="loading" class="flex justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 animate-spin">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.324.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 011.37.49l1.296 2.247a1.125 1.125 0 01-.26 1.431l-1.003.827c-.293.24-.438.613-.431.992a6.759 6.759 0 010 .255c-.007.378.138.75.43.99l1.005.828c.424.35.534.954.26 1.43l-1.298 2.247a1.125 1.125 0 01-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.57 6.57 0 01-.22.128c-.331.183-.581.495-.644.869l-.213 1.28c-.09.543-.56.941-1.11.941h-2.594c-.55 0-1.02-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 01-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 01-1.369-.49l-1.297-2.247a1.125 1.125 0 01.26-1.431l1.004-.827c.292-.24.437-.613.43-.992a6.932 6.932 0 010-.255c.007-.378-.138-.75-.43-.99l-1.004-.828a1.125 1.125 0 01-.26-1.43l1.297-2.247a1.125 1.125 0 011.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.087.22-.128.332-.183.582-.495.644-.869l.214-1.281z" />
            <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
        </div>

      <div class="v-else">
        <div v-if="surahs.hasOwnProperty('ayahs')" class="text-4xl">
          <!-- Lorem ipsum, dolor sit amet consectetur adipisicing elit. Adipisci totam non vitae perspiciatis facilis dolores, iste magnam ullam vel asperiores natus soluta eaque! Eligendi rem facere deserunt ratione nostrum ullam. -->
          <p class="flex items-center mb-6" v-for="ayah in surahs.ayahs" :key="ayah.number">
            <span class="text-xs w-5 h-5 flex items-center justify-center border rounded-full mr-4">{{ayah.numberInSurah}}</span> {{ayah.text}}</p>
        </div>
      </div>
        <!-- <div v-for="todo in todos">{{todo.title}}</div> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

 export default{
  name:'App',
  data(){
    return{
      allSurahs: [],
      surahs:[],
      loading:true

    }
  },
  mounted(){
    axios.get('https://api.alquran.cloud/v1/surah')
    .then(response => {
      this.allSurahs = response.data.data
    })

     this.querySpecificSurah(1);
    
   
  },
  methods: {
    getSpecificSurah(e) {
      this.querySpecificSurah(e.target.value)
      this.loading = true
    },
    querySpecificSurah(surahNumber){
      axios.get('https://api.alquran.cloud/v1/surah/'+surahNumber)
    .then(response => {    
      this.surahs = response.data.data
      this.loading = false;
    })
  }
}
 };

 //const { data: allSurahs } = await $fetch('https://api.alquran.cloud/v1/surah');

 //const { data: surahs }  = await $fetch('https://api.alquran.cloud/v1/surah/1/asad');
 


 
 
</script>