<template>
    <div class="col-12">
        <div class="card">
            <Toast />
            <Toolbar class="mb-4">
                <template v-slot:start>
                    <div class="my-2">
                        <Button 
                            label="Novo Item" 
                            icon="pi pi-plus" 
                            class="p-button-success mr-2" 
                            @click="openNew" />

                    </div>
                </template>
            </Toolbar>

            <DataTable 
                ref="dt" 
                :value="items" 
                dataKey="description" 
                v-model:selection="selectedItems" 
                :paginator="true"
                :rows="10" 
                :filters="filters"
                paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
                :rowsPerPageOptions="[5, 10, 25]"
                currentPageReportTemplate="Mostrando {first} de {last}, para um total {totalRecords} itens"
                responsiveLayout="scroll">

                <template #header>
                    <div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">
                        <h5 class="m-0">Itens de Avaliação</h5>
                        <span class="block mt-2 md:mt-0 p-input-icon-left">
                            <i class="pi pi-search" />
                            <InputText v-model="filters['global'].value" placeholder="Buscar..." />
                        </span>
                    </div>
                </template>

                <Column 
                    field="description" 
                    header="Descrição" 
                    :sortable="true" 
                    headerStyle="width:30%; min-width:10rem;">

                    <template #body="slotProps">
                        <span class="p-column-title capitalize">descrição</span>
                        <span class="capitalize">{{ slotProps.data.description }}</span> 
                    </template>
                </Column>

                <Column 
                    field="file" 
                    header="Comprovante" 
                    :sortable="true" 
                    headerStyle="width:30%; min-width:10rem;">

                    <template #body="slotProps">
                        <span class="p-column-title capitalize">comprovante</span>
                        {{ slotProps.data.file }}
                    </template>
                </Column>

                <Column 
                    field="points" 
                    header="Pontos" 
                    :sortable="true" 
                    headerStyle="width:30%; min-width:8rem;">

                    <template #body="slotProps">
                        <span class="p-column-title capitalize">pontos</span>
                        {{ slotProps.data.points }}
                    </template>
                </Column>

                <Column 
                    field="itemType" 
                    header="Item" 
                    :sortable="true" 
                    headerStyle="width:30%; min-width:10rem;">

                    <template #body="slotProps">
                        <span class="p-column-title capitalize">item</span>
                        <span class="capitalize">{{slotProps.data.itemType}}</span>
                    </template>
                </Column>

                <Column 
                    field="category" 
                    header="Fator" 
                    :sortable="true" 
                    headerStyle="width:30%; min-width:10rem;">

                    <template #body="slotProps">
                        <span class="p-column-title capitalize">fator</span>
                        <span class="capitalize">{{slotProps.data.category}}</span>
                    </template>
                </Column>

                <Column headerStyle="min-width:10rem;">
                    <template #body="slotProps">

                        <Button 
                            icon="pi pi-trash" 
                            class="p-button-rounded p-button-warning mt-2"
                            @click="confirmDeleteItem(slotProps.data)" />
                    </template>
                </Column>
            </DataTable>

            <Dialog 
                v-model:visible="showItemDialog" 
                :style="{ width: '450px' }" 
                header="Detalhes do Item" 
                :modal="true"
                class="p-fluid">

                <div class="field">
                    <label class="mb-3">Fator</label>
                    <div class="formgrid grid">

                        <div class="field-radiobutton col-6">
                            <RadioButton 
                                id="category1" 
                                name="category" 
                                value="Atualização Continuada" 
                                v-model="item.category" />
                            <label for="category1">Atualização Continuada</label>
                        </div>

                        <div class="field-radiobutton col-6">
                            <RadioButton 
                                id="category2" 
                                name="category" 
                                value="Funcional Pedagógico" 
                                v-model="item.category" />
                            <label for="category2">Funcional Pedagógico</label>
                        </div>

                        <div class="field-radiobutton col-6">
                            <RadioButton 
                                id="category3" 
                                name="category" 
                                value="Produção Institucional" 
                                v-model="item.category" />
                            <label for="category3">Produção Institucional</label>
                        </div>
                    </div>
                </div>


                <div class="field">
                    <label for="itemType" class="mb-3">Item</label>
                    <Dropdown 
                        id="itemType" 
                        v-model="item.itemType" 
                        :options="itemTypeList" 
                        optionLabel="label"
                        placeholder="Selecione um item">
                        <template #value="slotProps">
                            <div v-if="slotProps.value && slotProps.value.value">
                                <span :class="'product-badge status-' + slotProps.value.value">{{
                                        slotProps.value.label
                                }}</span>
                            </div>
                            <div v-else-if="slotProps.value && !slotProps.value.value">
                                <span :class="'product-badge status-' + slotProps.value.toLowerCase()">{{
                                        slotProps.value
                                }}</span>
                            </div>
                            <span v-else>
                                {{ slotProps.placeholder }}
                            </span>
                        </template>
                    </Dropdown>
                </div>

                <div class="field">
                    <label for="description" class="capitalize">descrição</label>
                    <Textarea 
                        id="description" 
                        v-model="item.description" 
                        required="true" 
                        rows="3" 
                        cols="20" />
                </div>

                <div class="field">
                    <label for="points" class="capitalize">pontos</label>
                    <InputNumber id="points" v-model="item.points" integeronly />
                </div>

                <div class="field">
                    <label for="file" class="capitalize">Comprovante</label>
                    <FileUpload 
                        id="file" 
                        mode="basic" 
                        name="demo[]" 
                        url="./upload.php" 
                        accept="image/*" 
                        :maxFileSize="1000000" 
                        @upload="onUpload" />
                </div>

                <template #footer>
                    <Button 
                        label="Cancelar" 
                        icon="pi pi-times" 
                        class="p-button-text" 
                        @click="hideDialog" />

                    <Button 
                        label="Salvar" 
                        icon="pi pi-check" 
                        class="p-button-text" 
                        @click="saveProduct" />
                </template>
            </Dialog>

            <Dialog 
                v-model:visible="deleteItemDialog" 
                :style="{ width: '450px' }" 
                header="Confirmação" 
                :modal="true">

                <div class="flex align-items-center justify-content-center">
                    <i class="pi pi-exclamation-triangle mr-3" style="font-size: 2rem" />
                    <span v-if="item">Tem certeza que deseja apagar <b>{{ item.description }}</b>?</span>
                </div>
                <template #footer>
                    <Button label="Não" icon="pi pi-times" class="p-button-text" @click="deleteItemDialog = false" />
                    <Button label="Sim" icon="pi pi-check" class="p-button-text" @click="deleteItem" />
                </template>
            </Dialog>

        </div>
    </div>
</template>

<script setup>
import { FilterMatchMode } from 'primevue/api';
import { onMounted, ref } from 'vue';
import { useToast } from "primevue/usetoast";

import ItemService from './ItemService';

const items = ref(null)
const item = ref({})
const showItemDialog = ref(false)
const deleteItemDialog = ref(false)
const selectedItems = ref([])
const itemTypeList = ref([
                            { label: 'Item 1', value: 'Item 1'},
                            { label: 'Item 2', value: 'Item 2'},
                            { label: 'Item 3', value: 'Item 3'}
                        ])
const filters = ref({})
const submitted = ref(false)

const itemService = new ItemService();


filters.value = {
    'global': { value: null, matchMode: FilterMatchMode.CONTAINS },
}

onMounted(() => itemService.getProducts().then(data => items.value = data))

const openNew = () => {
    item.value = {};
    submitted.value = false;
    showItemDialog.value = true;
}

const hideDialog = () => {
    showItemDialog.value = false;
    submitted.value = false;
}

const saveProduct = () => {
    submitted.value = true;
    if (this.product.name.trim()) {
        if (this.product.id) {
            this.product.inventoryStatus = this.product.inventoryStatus.value ? this.product.inventoryStatus.value : this.product.inventoryStatus;
            this.products[this.findIndexById(this.product.id)] = this.product;
            this.$toast.add({ severity: 'success', summary: 'Successful', detail: 'Product Updated', life: 3000 });
        }
        else {
            item.value.id = this.createId();
            item.value.code = this.createId();
            item.value.image = 'product-placeholder.svg';
            item.value.inventoryStatus = this.product.inventoryStatus ? this.product.inventoryStatus.value : 'INSTOCK';
            item.value.push(item.value);
            toast.add({ severity: 'success', summary: 'Successful', detail: 'Product Created', life: 3000 });
        }
        showItemDialog.value = false;
        item.value = {};
    }
}

const confirmDeleteItem = (item) => {
    item.value = item;
    deleteItemDialog.value = true;
}

const toast = useToast();

const deleteItem = () => {
    items.value = items.value.filter(val => val.id !== item.value.id);
    deleteItemDialog.value = false;
    item.value = {};
    toast.add(
        { 
            severity: 'success', 
            summary: 'Sucesso', 
            detail: 'Item apagado', life: 3000 
        });
}

</script>
