<script setup>
import { ref } from "vue";

const slike = {
  Jabuka: "https://www.svgrepo.com/show/530203/apple.svg",
  Mrkva: "https://www.svgrepo.com/show/530216/carrot.svg",
  Sir: "https://www.svgrepo.com/show/530219/cake.svg",
  Kruh: "https://www.svgrepo.com/show/530223/bread.svg",
};

const proizvodi = [
  {
    naziv: "Jabuka",
    cijena: 0.25,
  },
  {
    naziv: "Mrkva",
    cijena: 0.12,
  },
  {
    naziv: "Kruh",
    cijena: 2.0,
  },
  {
    naziv: "Sir",
    cijena: 4.48,
  },
];

const korisnik = {
  jeAdmin: true,
  osobni_podaci: {
    ime: "Marko",
    prezime: "Krivić",
    adresa: {
      grad: "Pula",
      ulica: "Veruda",
      broj: 32,
    },
    broj_telefona: "+091-123-456",
  },
  kosarica: [
    {
      naziv: "Jabuka",
      količina: 4,
    },
    {
      naziv: "Mrkva",
      količina: 12,
    },
    {
      naziv: "Sir",
      količina: 1,
    },
    {
      naziv: "Kruh",
      količina: 3,
    },
  ],
};

const itemClass = () => "bg-white p-8 text-black w-full flex flex-row m-auto rounded-lg mb-8 gap-8";


function dohvatiCijenu (naziv) {
  let stavkaID = proizvodi.findIndex(p => p.naziv === naziv);
  if (stavkaID != -1) {
    return proizvodi[stavkaID].cijena;
  } else {
    return 0;
  }
}

function sveukupnaCijena() {
  let ukupno = 0;
  for (let i = 0; i < korisnik.kosarica.length; i++) {
    let cijena = dohvatiCijenu(korisnik.kosarica[i].naziv);
    if (cijena != 0) {
      ukupno += cijena * korisnik.kosarica[i].količina;
    }
  }
  return ukupno;
}

function najskupljaStavka(){
  let najskuplja = korisnik.kosarica[0].naziv;
  let maxCijena = korisnik.kosarica[0].količina * dohvatiCijenu(korisnik.kosarica[0].naziv);

  for(let i = 1; i < korisnik.kosarica.length; i++){
    let trenutnaStavka = korisnik.kosarica[i].količina * dohvatiCijenu(korisnik.kosarica[i].naziv);
    if(trenutnaStavka > maxCijena){
      maxCijena = trenutnaStavka;
      najskuplja = korisnik.kosarica[i].naziv;
    }    	
  }
  return najskuplja;
}
</script>

<template>
  <div class="bg-slate-100 p-8 text-blue-600 w-5/10 flex flex-col m-auto rounded-lg shadow-lg mb-8">
    <p class="font-bold text-2xl mb-4 border-b-1 border-blue-500/25 pb-2">Korisnički podaci</p>
    <p>
      <span class="font-bold">Ime:</span> <span :class="korisnik.jeAdmin ? 'text-blue-600' : 'text-black'"> {{ korisnik.osobni_podaci.ime }} {{ korisnik.osobni_podaci.prezime }} </span>
    </p>
    <p><span class="font-bold">Adresa:</span> {{ korisnik.osobni_podaci.adresa.ulica }} {{ korisnik.osobni_podaci.adresa.broj }},  {{ korisnik.osobni_podaci.adresa.grad }}</p>
    <p>
      <span class="font-bold">Telefon:</span> {{ korisnik.osobni_podaci.broj_telefona }}
    </p>
  </div>

  <div class="bg-slate-100 p-8 text-black w-5/10 flex flex-col m-auto rounded-lg shadow-lg mb-8">
    <p class="font-bold text-2xl mb-4">Košarica</p>
    <div :class="[najskupljaStavka() === 'Jabuka' ? 'bg-red-200 border border-red-500 p-8 w-full flex flex-row m-auto rounded-lg mb-8 gap-8' : itemClass()]">
    <div>
      <img :src="slike.Jabuka" alt="Jabuka" class="w-16 h-16 mr-4" />
    </div>
    <ul class="list-none text-gray-600">
      <li class="font-bold text-xl mb-1">{{ korisnik.kosarica[0].naziv }}</li>
      <li>Cijena: €{{ dohvatiCijenu('Jabuka') }} | Količina: {{ korisnik.kosarica[0].količina }}</li>
      <li>Ukupno: €{{ dohvatiCijenu('Jabuka') * korisnik.kosarica[0].količina }}</li>
    </ul>    
  </div>
  <div :class="[najskupljaStavka() === 'Mrkva' ? 'bg-red-200 border border-red-500 p-8 w-full flex flex-row m-auto rounded-lg mb-8 gap-8' : itemClass()]">
    <div>
      <img :src="slike.Mrkva" alt="Mrkva" class="w-16 h-16 mr-4" />
    </div>
    <ul class="list-none text-gray-600">
      <li class="font-bold text-xl mb-1">{{ korisnik.kosarica[1].naziv }}</li>
      <li>Cijena: €{{ dohvatiCijenu('Mrkva') }} | Količina: {{ korisnik.kosarica[1].količina }}</li>
      <li>Ukupno: €{{ dohvatiCijenu('Mrkva') * korisnik.kosarica[1].količina }}</li>
    </ul>  
  </div>
  <div :class="[najskupljaStavka() === 'Sir' ? 'bg-red-200 border border-red-500 p-8 w-full flex flex-row m-auto rounded-lg mb-8 gap-8' : itemClass()]">
    <div>
      <img :src="slike.Sir" alt="Sir" class="w-16 h-16 mr-4" />
    </div>
    <ul class="list-none text-gray-600">
      <li class="font-bold text-xl mb-1">{{ korisnik.kosarica[2].naziv }}</li>
      <li>Cijena: €{{ dohvatiCijenu('Sir') }} | Količina: {{ korisnik.kosarica[2].količina }}</li>
      <li>Ukupno: €{{ dohvatiCijenu('Sir') * korisnik.kosarica[2].količina }}</li>
    </ul>
  </div>
  <div :class="[najskupljaStavka() === 'Kruh' ? 'bg-red-200 border border-red-600 p-8 w-full flex flex-row m-auto rounded-lg mb-8 gap-8' : itemClass()]">
    <div>
      <img :src="slike.Kruh" alt="Kruh" class="w-16 h-16 mr-4" />
    </div>
    <ul class="list-none text-gray-600">
      <li class="font-bold text-xl mb-1">{{ korisnik.kosarica[3].naziv }}</li>
      <li>Cijena: €{{ dohvatiCijenu('Kruh') }} | Količina: {{ korisnik.kosarica[3].količina }}</li>
      <li>Ukupno: €{{ dohvatiCijenu('Kruh') * korisnik.kosarica[3].količina }}</li>
    </ul>
  </div>
  <div class="text-2xl">
    <span class="font-bold">Ukupna Cijena:</span> €{{ sveukupnaCijena() }}
  </div>
  </div>  
</template>

<style scoped></style>
