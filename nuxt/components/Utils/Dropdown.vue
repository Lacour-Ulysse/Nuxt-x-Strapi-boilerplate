<template>
  <div class="relative">
    <Menu as="div" class="relative text-left">
      <MenuButton
        :class="[
          'flex',
          'items-center',
          'btn',
          {
            'btn-default': variant === 'default',
            'btn-primary': variant === 'primary',
            'btn-outline': variant === 'outline',
            'btn-danger': variant === 'danger',
          },
        ]"
      >
        <span>{{ buttonLabel }}</span>
        <UIcon name="i-heroicons-chevron-down-20-solid" />
      </MenuButton>

      <transition
        enter-active-class="transition duration-100 ease-out"
        enter-from-class="transform scale-95 opacity-0"
        enter-to-class="transform scale-100 opacity-100"
        leave-active-class="transition duration-75 ease-in"
        leave-from-class="transform scale-100 opacity-100"
        leave-to-class="transform scale-95 opacity-0"
      >
        <MenuItems
          class="absolute right-0 z-10 w-full mt-2 origin-top-right bg-white divide-y divide-gray-100 rounded-md shadow-lg dark:bg-gray-800 ring-1 ring-black ring-opacity-5 focus:outline-none"
        >
          <div class="px-1 py-1">
            <MenuItem
              v-for="(item, itemIdx) in menuItems"
              :key="itemIdx"
              v-slot="{ active }"
            >
              <button
                type="button"
                @click="handleMenuItemClick(item)"
                :class="[
                  active
                    ? 'bg-primary-500 dark:bg-primary-800'
                    : 'text-gray-900 dark:text-gray-100',
                  'group flex rounded-md items-center w-full px-2 py-2 text-sm',
                ]"
              >
                {{ item.label }}
              </button>
            </MenuItem>
          </div>
        </MenuItems>
      </transition>
    </Menu>
  </div>
</template>

<script setup>
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue";
// import { ChevronDownIcon } from "@heroicons/vue/24/solid";

const props = defineProps({
  buttonLabel: {
    type: String,
    default: "Actions",
  },
  variant: {
    type: String,
    default: "primary",
  },
  menuItems: {
    type: Array,
    default: () => [
      {
        label: "Action",
        onClick: () => {},
      },
    ],
    validator: (value) => {
      return value.every((item) => {
        return item.hasOwnProperty("label") && item.hasOwnProperty("action");
      });
    },
  },
});
function handleMenuItemClick(item) {
  item.action();
}
</script>
