<script setup lang="ts">
import CarComponent from "src/components/CarCards.vue";
import { useStore } from "src/stores/store";
import { ref, watch } from "vue";
import { onMounted } from "vue";
import EditDialog from "../components/EditDialogComponent.vue"

const store = useStore();
const firstSelectedCategory = ref("Személyautó");
const list = ref([]);
onMounted(() => {
  store.getCategories();
  store.one_GetAll();
  store.getAllCarsByCategory(firstSelectedCategory.value);
  selectionChanged();
});
const selectionChanged = () => {
  store.getAllCarsByCategory(firstSelectedCategory.value);
};
function editDocument(ad) {
  store.many.document._id = ad._id;
  store.app.showEditDialog = true;
}
watch(
  () => store.other.documents,
  (newDocuments) => {
    list.value = newDocuments;
  },
);
</script>
<template>
  <q-page>
    <div class="row justify-center">
      <q-select
        v-model="firstSelectedCategory"
        emit-value
        label="Kategória"
        map-options
        option-label="nev"
        option-value="nev"
        :options="store.many.documents"
        @update:model-value="selectionChanged()"
      ></q-select>
    </div>
    <div class="row justify-center q-ma-xl">
      <div v-for="(item, index) in list" :key="index" class="col-xs-12 col-sm-6 col-md-4 col-lg-3">
        <CarComponent :index="index" @editDialog="editDocument(item)"></CarComponent>
      </div>
    </div>
  </q-page>
  <EditDialog />
</template>
<style lang="scss" scoped></style>
