<template>
	<div class="dropdown-input">
		<div class="dropdown-box" :style="{ minWidth: `${minWidth}px` }" @click="clickDropdownBox" data-hover-menu-spawner>
			<Icon :class="'dropdown-icon'" :icon="activeEntry.icon" v-if="activeEntry.icon" />
			<span>{{ activeEntry.label }}</span>
			<Icon :class="'dropdown-arrow'" :icon="'DropdownArrow'" />
		</div>
		<MenuList
			:menuEntries="menuEntries"
			:activeEntry="activeEntry"
			:defaultAction="setActiveEntry"
			:direction="MenuDirection.Bottom"
			:widthChanged="widthChanged"
			:drawIcon="drawIcon"
			ref="menuList"
		/>
	</div>
</template>

<style lang="scss">
.dropdown-input {
	position: relative;

	.dropdown-box {
		display: flex;
		align-items: center;
		white-space: nowrap;
		background: var(--color-1-nearblack);
		height: 24px;
		border-radius: 2px;

		.dropdown-icon {
			margin: 4px;
			flex: 0 0 auto;
		}

		span {
			display: inline-block;
			margin: 0;
			margin-left: 8px;
			flex: 1 1 100%;
		}

		.dropdown-icon + span {
			margin-left: 0;
		}

		.dropdown-arrow {
			margin: 6px 2px;
			flex: 0 0 auto;
		}

		&:hover,
		&.open {
			background: var(--color-6-lowergray);

			span {
				color: var(--color-f-white);
			}

			svg {
				fill: var(--color-f-white);
			}
		}

		&.open {
			border-radius: 2px 2px 0 0;
		}
	}

	.menu-list .floating-menu-container .floating-menu-content {
		max-height: 400px;
		overflow-y: auto;
	}
}
</style>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import Icon from "../labels/Icon.vue";
import MenuList, { MenuListEntry, SectionsOfMenuListEntries } from "../floating-menus/MenuList.vue";
import { MenuDirection } from "../floating-menus/FloatingMenu.vue";

export default defineComponent({
	props: {
		menuEntries: { type: Array as PropType<SectionsOfMenuListEntries>, required: true },
		default: { type: Object as PropType<MenuListEntry>, required: true },
		drawIcon: { type: Boolean, default: false },
	},
	data() {
		return {
			activeEntry: this.default,
			MenuDirection,
			minWidth: 0,
		};
	},
	methods: {
		clickDropdownBox() {
			(this.$refs.menuList as typeof MenuList).setOpen();
		},
		setActiveEntry(newActiveEntry: MenuListEntry) {
			this.activeEntry = newActiveEntry;
		},
		widthChanged(newWidth: number) {
			this.minWidth = newWidth;
		},
	},
	components: {
		Icon,
		MenuList,
	},
});
</script>
