<template>
	<div class="grid">
		<stats-widget 
			v-for="item in statsItems" 
			:key="item.title" 
			:title="item.title"
			:currentPoints="item.currentPoints" 
			:maxPoints="item.maxPoints" 
			:icon="item.icon"
			:iconBackgroundColour="item.iconBackgroundColour" 
			:iconTextColour="item.iconTextColour">
		</stats-widget>

		<table-items></table-items>

	</div>
</template>

<script setup>
import { ref } from 'vue';
import StatsWidget from './StatsWidget.vue'
import TableItems from './TableItems.vue'

const statsItems = ref([
	{
		title: "atualização continuada",
		currentPoints: 15,
		maxPoints: 30,
		icon: "pi pi-chart-line",
		iconBackgroundColour: "bg-blue-100",
		iconTextColour: "text-blue-500"
	},
	{
		title: "funcional pedagógico",
		currentPoints: 35,
		maxPoints: 30,
		icon: "pi pi-briefcase",
		iconBackgroundColour: "bg-orange-100",
		iconTextColour: "text-orange-500"
	},
	{
		title: "produção institucional",
		currentPoints: 20,
		maxPoints: 30,
		icon: "pi pi-book",
		iconBackgroundColour: "bg-cyan-100",
		iconTextColour: "text-cyan-500"
	},
])
</script>

