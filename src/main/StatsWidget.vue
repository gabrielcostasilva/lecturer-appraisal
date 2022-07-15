<template>
    <div class="hidden md:block md:col-4">
        <div class="card mb-0">
            <div class="flex justify-content-between mb-3">
                <div>
                    <span class="block text-500 font-medium mb-3 uppercase">{{ title }}</span>
                    <div class="text-900 font-medium text-xl">{{ currentPoints }} / {{ maxPoints }}</div>
                </div>
                <div :class="iconBackgroundColour" class="flex align-items-center justify-content-center border-round"
                    style="width:2.5rem;height:2.5rem">
                    <i :class="[icon, iconTextColour]" class="text-xl"></i>
                </div>
            </div>
            <div class="col">
                <ProgressBar :value="progressBarValue" :showValue="false" style="height: .5em"></ProgressBar>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, computed } from 'vue'

const props = defineProps([
    'title', 
    'icon', 
    'iconBackgroundColour', 
    'iconTextColour', 
    'currentPoints', 
    'maxPoints'
])

const progressBarValue = computed(() => props.currentPoints / props.maxPoints * 100)

</script>
