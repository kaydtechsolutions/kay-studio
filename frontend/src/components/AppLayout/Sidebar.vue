<template>
	<div
		:class="[
			'sticky bottom-0 left-0 top-0 flex h-full shrink-0 flex-col bg-gray-50 px-2 pt-2 transition-all',
			isCollapsed ? '!w-13' : 'w-60'
		]"
	>
		<button :class="['mb-1 flex w-full gap-2 rounded p-2 hover:bg-gray-200', isCollapsed ? 'flex-col' : 'items-center justify-between']">
			<slot name="header">
				<div class="rounded-sm">
					<div v-if="logoSVG" class="flex items-center gap-2">
						<span v-html="logoSVG" />
					</div>
					<AppLogo v-else class="h-6 w-6" />
				</div>

				<span v-if="!isCollapsed" class="truncate text-xl font-bold text-gray-800">
					{{ title }}
				</span>
				<IconButton :icon="isCollapsed ? 'chevrons-right' : 'chevrons-left'" label="Collapse" @click="toggleCollapse" />
			</slot>
		</button>

		<nav class="mt-2 flex flex-1 flex-col space-y-1 overflow-y-auto">
			<div class="w-full" v-for="item in menuItems" :key="item.label">
				<component :is="item.route_to ? 'router-link' : 'div'" :to="item.route_to" class="flex cursor-pointer items-center gap-2 truncate rounded px-2 py-1 transition duration-300 ease-in-out" :class="[
					$router.currentRoute.value.path === item.route_to ? 'bg-white shadow-sm' : 'hover:bg-gray-200',
				]" @click="item.route_to && $router.push(item.route_to)">
					<FeatherIcon :name="item.featherIcon || 'folder-normal'" class="h-5 w-5 text-gray-700" />
					<div v-if="!isCollapsed" class="flex items-center gap-1 truncate text-base text-gray-700">
						{{ item.label }}
					</div>
				</component>
			</div>
		</nav>
	</div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import IconButton from "../IconButton.vue";
import AppLogo from "../Icons/AppLogo.vue";
import { SidebarProps } from "@/types/studio_components/Sidebar";

withDefaults(defineProps<SidebarProps>(), {
	menuItems: () => [],
});

// Reactive variable for collapse state
const isCollapsed = ref(false);

// Function to toggle collapse state
const toggleCollapse = () => {
	isCollapsed.value = !isCollapsed.value;
};
</script>
