<template>
  <common-modal
      :title="`${props.title} 수정`"
      :width="700"
      :open="isOpen"
      @input="close"
  >
    <template v-slot:contents>
      <slot name="contents" :setEntityDTO="setEntityDTO"></slot>
    </template>
    <template v-slot:buttons>
      <v-spacer></v-spacer>
      <v-btn @click="save" variant="text" color="secondary">저장</v-btn>
      <v-btn @click="close" variant="text">취소</v-btn>
    </template>
  </common-modal>
</template>
<script setup>
import {defineProps, defineEmits, ref, toRaw, watch} from "vue";
import axios from 'axios';
import api from "@/components/common/utils/httpUtil";
import CommonModal from "@/components/common/modals/CommonModal";
import {useCredentialStore} from "@/stores/credential/credentialStore";

const emit = defineEmits(['input', 'finished', 'check']);
const props = defineProps({open: Boolean, entity: Object, title: String, endpoint: String});
const store = useCredentialStore();
const entityDTO = ref(null);
let isOpen = ref(props.open);


watch(() => props.open, () => {isOpen.value = props.open;});

function setEntityDTO(commonEntity) {
    entityDTO.value = commonEntity;
}

async function save() {
    await emit('check', async () => {await reqUpdateEntity()});
}

async function reqUpdateEntity() {
    entityDTO.value['id'] = props.entity['id'];
    console.log(toRaw(entityDTO.value));
    try {
        await axios.put(`${api.SERVER_URL}${props.endpoint}`, toRaw(entityDTO.value), {
            headers: {Authorization: store['token']}
        });
        emit('finished');
        alert(`${props.title} 업데이트가 완료되었습니다.`);
        close();
    } catch (error) {
        api.handleHttpError(error);
    }
}

function close() {
    isOpen.value = false;
    emit('input', false);
}

</script>
