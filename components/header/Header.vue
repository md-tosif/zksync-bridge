<template>
  <div class="navbar">
    <Popover class="popover-container" v-slot="{ open }">
      <PopoverFunctional :open="open" @popupToggled="handlePopupToggled" />
      <div
        class="panel-container-desktop"
        :class="{ 'bg-gray dark:bg-black lg:bg-transparent dark:lg:bg-transparent': open }"
      >
        <div class="logo-container">
          <NuxtLink v-if="logoLeadsHome" :to="{ name: 'index' }">
            <IconsZkSync class="w-[120px]" />
          </NuxtLink>
          <a v-else href="https://zeeve.io" target="_blank">
            <IconsZkSync class="w-[120px]" />
          </a>
        </div>
        <div class="right-side-menu">
          <RightSideMenu />
        </div>
        <div class="popover-button left-border">
          <PopoverButton class="align-bottom focus-visible:outline-none" aria-label="Toggle menu">
            <Bars3Icon v-if="!open" class="menu-icon" />
            <XMarkIcon v-else class="menu-icon" />
          </PopoverButton>
        </div>
      </div>
    </Popover>
  </div>
</template>

<script lang="ts" setup>
import { computed, reactive } from "vue";

import { Popover, PopoverButton, PopoverGroup, PopoverPanel } from "@headlessui/vue";
import { Bars3Icon, XMarkIcon } from "@heroicons/vue/24/outline";
import { storeToRefs } from "pinia";

import DropdownContent from "./DropdownContent.vue";
import DropdownPopover from "./DropdownPopover.vue";
import PopoverFunctional from "./PopoverFunctional.vue";
import RightSideMenu from "./RightSideMenu.vue";

import { tabs as bridgeTabs } from "@/components/bridge/Navigation.vue";

import { useRoute } from "#app";
import { useOnboardStore } from "@/store/onboard";

const route = useRoute();
const { account } = storeToRefs(useOnboardStore());
const logoLeadsHome = computed(() => {
  return !bridgeTabs.map((e) => e.routeName).includes(route.name as string) && account.value.address;
});

export interface Navigation {
  title?: string;
  items: {
    label: string;
    url: string;
  }[];
}

const emit = defineEmits<{
  (eventName: "update:popupOpened", value: boolean): void;
}>();
function handlePopupToggled(value: boolean) {
  emit("update:popupOpened", value);
}

const learnNav: Navigation[] = reactive([
  {
    title: "",
    items: [
      { label: "Freedom is our mission", url: "" },
      { label: "Hyperscalibility", url: "" },
      { label: "Security", url: "" },
      { label: "Game-changing UX", url: "" },
    ],
  },
]);

const buildNav: Navigation[] = reactive([
  {
    title: "",
    items: [
      { label: "Quickstart", url: "" },
      { label: "Documentation", url: "" },
      { label: "Web3 API", url: "" },
    ],
  },
  {
    title: "Guides",
    items: [
      {
        label: "Contract deployment",
        url: "",
      },
      { label: "Bridging assets", url: "" },
      { label: "Account abstraction", url: "" },
      {
        label: "Building custom Paymasters",
        url: "",
      },
      { label: "Cross-chain governance", url: "" },
    ],
  },
  {
    title: "Tools",
    items: [
      { label: "Javascript SDK", url: "" },
      { label: "Hardhat plugins", url: "" },
      { label: "zkSync Era CLI", url: "" },
    ],
  },
]);

const networkNav: Navigation[] = reactive([
  {
    title: "zkSync Era (v2)",
    items: [
      { label: "Intro to zkSync Era", url: "" },
      { label: "Wallet Portal", url: "" },
      { label: "Block Explorer", url: "" },
    ],
  },
  {
    title: "zkSync Lite (v1)",
    items: [
      { label: "Intro to zkSync Lite", url: "" },
      { label: "Wallet Portal", url: "" },
      { label: "Block Explorer", url: "" },
    ],
  },
  {
    title: "Ecosystem",
    items: [
      { label: "Explore the Ecosystem", url: "" },
      {
        label: "Brand assets",
        url: "",
      },
    ],
  },
]);
</script>
<style lang="scss">
.navbar {
  @apply z-50 h-[72px] lg:mb-3;
  grid-area: header / header / header / header;

  .popover-container {
    @apply h-full text-neutral-950 dark:text-white;
  }
  .panel-container-desktop {
    @apply relative z-20 mx-auto flex h-full items-center justify-between px-5;
    @media screen and (min-width: 720px) {
      @apply px-6;
    }
    @media screen and (min-width: 1024px) {
      @apply grid grid-cols-[216px_1fr_216px] px-9;
    }

    .logo-container {
      @apply font-semibold;
    }
    .navigation-container {
      @apply hidden h-full justify-self-center lg:flex;
      .popover-group {
        @apply flex;
      }
    }
    .right-side-menu {
      @apply hidden lg:block;
    }
    .popover-button {
      @apply z-10 -mr-4 p-4 focus:outline-none focus-visible:outline-none lg:hidden;
      .menu-icon {
        @apply w-6 focus:outline-none;
      }
    }
  }
  .panel-container-mobile {
    @apply absolute left-0 z-50 w-full bg-gray px-6 dark:bg-black lg:hidden;
    .menu-label {
      @apply pb-2 pt-8 first:pt-0;
    }
    .menu-section-container {
      @apply mb-8 flex-wrap justify-between border-b pb-8 dark:border-[#292B43] xs:justify-start sm:flex-nowrap sm:justify-between;
      .menu-section {
        @apply min-w-[100px] max-w-[6rem] xxs:min-w-[132px] xxs:max-w-[8.25rem] xs:min-w-[160px] xs:max-w-[11rem] sm:min-w-[27%] sm:max-w-[21rem];
      }
    }
    .were-hiring {
      @apply mb-8 border-b border-[#292B43] pb-8 font-extralight;
    }
    .v1-docs {
      @apply pb-10 font-extralight;
    }
  }
}
</style>
