<template>
  <v-layout>
    <header-fragment
        @login="openLoginModal"
    ></header-fragment>
    <v-main>
      <v-carousel class="carousel" :show-arrows="false" :hide-delimiters="true" :cycle="true" :interval="3000">
        <v-carousel-item
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
            :cover="true"
        ></v-carousel-item>
        <v-carousel-item
            src="https://cdn.vuetifyjs.com/images/cards/hotel.jpg"
            :cover="true"
        ></v-carousel-item>
        <v-carousel-item
            src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
            :cover="true"
        ></v-carousel-item>
        <div class="carousel_banner">
          <h1 class="carousel_text">사람의 기분을 인식하는 스마트 AI 뮤직 플레이어</h1>
          <h2 class="carousel_text">당신의 오늘 기분은 어떠신가요?</h2>
        </div>
      </v-carousel>

      <sentence-result-component v-if="store['mode'] === 'result'"
          @reset="changeCardMode('stand-by')"
      ></sentence-result-component>
      <sentence-input-component v-else
          @classify="changeCardMode('result')"
      ></sentence-input-component>

      <login-modal
          :open="loginModalIsOpen"
          @input="toggleLoginModal"
      ></login-modal>
    </v-main>
  </v-layout>
</template>

<script setup>
import HeaderFragment from '@/components/common/components/HeaderFragment';
import {ref} from 'vue';
import SentenceInputComponent from "@/components/main/components/SentenceInputComponent";
import SentenceResultComponent from "@/components/main/components/SentenceResultComponent";
import LoginModal from "@/components/main/modals/LoginModal";
import {useClassifyStore} from "@/stores/classify/classifyStore";


const store = useClassifyStore();
const loginModalIsOpen = ref(false);

function toggleLoginModal(status) {
  loginModalIsOpen.value = status ? status : false;
}
function openLoginModal() {
  loginModalIsOpen.value = true;
}

function changeCardMode(mode) {
  store['mode'] = mode;
  console.log(store['mode'].value);
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.carousel_banner {
  position: absolute;
  width: calc(100% - 50px);
  height: 80px;
  margin: 210px 25px;
  color: #ffffff;
  border-radius: 10px;
  background-color: rgba(0,0,0,0.5);
  text-align: center;
}
</style>
