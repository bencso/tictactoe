<script>
import { computed, ref } from 'vue'
export default {
  name: 'Játék',
  setup () {
    const jatekos = ref('X') 
    let kerdes = prompt("Üdvözlünk a játékunk oldalán!\nJátékos 1 , melyik betűvel szeretnél lenni (X/O/x/o)?\n(Amennyiben nem megfelelő betűt írsz X-el leszel)", "");
    kerdes.toLowerCase() == 'O'.toLowerCase() ? jatekos.value = 'O' : jatekos.value = 'X';; 
    const negyzet = ref([])
    negyzet.value = Array(9).fill('')
    const nyertes = computed(() => eldontes(negyzet.value.flat()))
    function eldontes (value) {
      const nyero = [
        [0, 1, 2],
        [0, 3, 6],
        [0, 4, 8],
        [1, 4, 7],
        [2, 5, 8], 
        [2, 4, 6],
        [3, 4, 5],
        [6, 7, 8]
      ]
      for (let i = 0; i < nyero.length; i++) {
        const [a, b, c] = nyero[i]
        if (
          value[a] &&
          value[a] === value[b] &&
          value[a] === value[c]
        ) {
          return value[a]
        }
      }
      return null
    }
    function kovetkezo(i) {
      if (nyertes.value) {
        return
      }
      if(negyzet.value[i]==""){
      negyzet.value[i] = jatekos.value
      jatekos.value = jatekos.value === 'X' ? 'O' : 'X'
      }
    }
    function ujra () {
      negyzet.value = Array(9).fill('')
    }
    return { negyzet, kovetkezo, nyertes, jatekos, ujra }
  }
}
</script>
<template>
<nav class="font-medium flex flex-col p-5 border md:flex-row md:space-x-8 rtl:space-x-reverse md:mt-0 md:border-0 mb-10 bg-gray-900 select-none">
  <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
    <a class="flex items-center space-x-3 rtl:space-x-reverse">
        <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">TikkTakkTó</span>
    </a>
    <div class="w-50 md:block md:w-auto">
      <ul class="grid md:gap-0 gap-2 md:grid-cols-2 grid-cols-1 font-medium ml-3 p-5 border border-gray-100 rounded-lg rtl:space-x-reverse md:mt-0 bg-white">
        <li class="ml-4 cursor-grab" v-if="nyertes"><span class="font-bold">Nyertes: </span>{{ nyertes }}</li>
        <li class="ml-4 cursor-grab" v-else><span class="font-bold">Játékos: </span>{{ jatekos }}</li>
      <li>
        <a v-if="nyertes" class="btn bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mt-5 cursor-pointer" @click="ujra">Kezdjük újra!</a>
        <a v-else="nyertes" class="btn bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded mt-5 cursor-pointer" @click="ujra">Játszunk újra!</a>
      </li>
      </ul>
    </div>
  </div>
</nav>
  <div id="palya" class="grid grid-cols-3 select-none">
    <div class="negyzet cursor-pointer" v-for="(negyzet, i) in negyzet" :key="i" @click="kovetkezo(i)">
      {{ negyzet }} 
    </div>
  </div>
</template>

<style scoped>
button{
  margin-top: 40px;
}
#palya {
  height: 315px;
  width: 310px;
  margin:0 auto;
}
.negyzet {
  border: 1px solid #999;
  font-size:100px;
  font-weight: bold;
  line-height: 90px;
  height: 100px;
  width: 100px;
  padding: 0;
  text-align: center;
}
</style>