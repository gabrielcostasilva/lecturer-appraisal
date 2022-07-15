<template>
	<div class="layout-topbar">
		<button class="p-link layout-menu-button layout-topbar-button" @click="onMenuToggle">
			<i class="pi pi-bars"></i>
		</button>

		<span class="layout-topbar-logo capitalize">john doe</span>

		<button class="p-link layout-topbar-menu-button layout-topbar-button" v-styleclass="{
			selector: '@next', enterClass: 'hidden', enterActiveClass: 'scalein',
			leaveToClass: 'hidden', leaveActiveClass: 'fadeout', hideOnOutsideClick: true
		}">
			<i class="pi pi-ellipsis-v"></i>
		</button>
		<ul class="layout-topbar-menu hidden lg:flex origin-top">
			<li>
				<button class="p-link layout-topbar-button">
					<i class="pi pi-calendar mr-2"></i>
					2021
				</button>
			</li>
			<li>
				<button class="p-link layout-topbar-button">
					<i class="pi pi-sign-out"></i>
				</button>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	methods: {
		onMenuToggle(event) {
			this.$emit('menu-toggle', event);
		},
		onTopbarMenuToggle(event) {
			this.$emit('topbar-menu-toggle', event);
		},
	},
	computed: {
		darkTheme() {
			return this.$appState.darkTheme;
		}
	}
}
</script>