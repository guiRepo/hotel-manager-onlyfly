<script setup lang="ts">
  import CardDetails from './CardDetails.vue'
  import { ref } from 'vue'
  import axios from 'axios';

  const props = defineProps<{ data: any }>()
  
  const dialog = ref(false);
  const hotelDetails = ref<any>(null);

  async function getItem() {
    try {
    const res = await axios.get(`http://localhost:3000/hotels_details?id=${props.data?.id}`)
      hotelDetails.value = res.data[0]
      dialog.value = true
      console.log(res.data[0])
    } catch (err) {
      console.error('Erro ao buscar hotéis:', err)
    }
  }
</script>

<template>
  <div class="pricing-container">
    <div class="text-caption text-grey">Por dia</div>
    <div class="text-h6 daily-pricing-text">R$ {{ props.data?.dailyPrice }}</div>

    <div class="details q-mt-sm">
      <div class="row-text">Diarias <span class="q-ml-sm">R$ {{ data?.dailyPrice }},00</span></div>
      <div class="row-text">Taxas <span class="q-ml-sm">R$ {{ data?.tax }},00</span></div>
      <div class="row-text">Total <span class="q-ml-sm">R$ {{ data?.totalPrice }}</span></div>
    </div>

    <q-btn label="Ver detalhes" class="q-mt-sm full-width details-button" rounded @click="getItem()" />

    <CardDetails v-model="dialog" :hotel="hotelDetails" :hotelName="data?.name"  :data="data"/>
  </div>
</template>

<style scoped lang="scss">
  .pricing-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 14px;
    width: 100%;
    padding: 10px;
  }

  .details {
    margin-top: 10px;
    color: #777;
    font-size: 13px;
  }

  .row-text {
    display: flex;
    justify-content: space-between;
    margin-bottom: 2px;
  }

  .details-button {
    background-color: $primary-100;
  }

  .daily-pricing-text {
    font-size: $font-size-title;
    color: $primary-100;
  }
</style>