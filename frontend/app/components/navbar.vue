<script setup lang="ts">
import SampleMenu from '~/pages/sample.vue';

</script>

<template>
<header class="sticky top-0 z-50 backdrop-blur-md px-4 lg:px-12 py-2">
    <div class="max-w-7xl mx-auto flex items-center justify-between">
      <!-- Brand Logo -->
      <a href="#" class="flex items-center space-x-3 group">
        <div class=" p-[2px] group-hover:scale-105 transition-transform flex items-center">
          <img src="/assets/AwazeLogo0.2.png" class=" w-20 h-8" alt="Logo">
        </div>
        <!-- Brand Logo <div class="flex flex-col">
          <span class="font-black text-xl tracking-tight text-white uppercase leading-none">AWAZE <span class="text-transparent text-[1px]">.</span><span class="text-brand-400 text-xl uppercase font-bold tracking-widest mt-1"> አዋዜ</span></span>
        </div>-->
      </a>

      <!-- Primary Action CTA -->
      <div class="flex gap-3">
        <NuxtLink to="menu/girum">
          <a href="../pages/menu/girum">
            <h1 class="flex items-center space-x-4 shimmer-button bg-brand-500 hover:bg-brand-400 text-center text-black font-black text-[9px] uppercase tracking-widest px-6 md:px-6 py-3 rounded-xl shadow-lg shadow-brand-500/20 transition-all active:scale-95 text-wrap w-32 h-full md:text-[12px] md:w-full ">See Sample Menu</h1>
          </a>
        </NuxtLink>
        <NuxtLink to="admin/dashboard">
          <a href="../pages/admin/dashboard">
            <h1 class="flex items-center space-x-4 shimmer-button bg-brand-500 hover:bg-brand-400 text-center text-black font-black text-[9px] text-wrap uppercase tracking-widest px-2 md:px-6 py-3 rounded-xl shadow-lg shadow-brand-500/20 transition-all active:scale-95 w-32 h-full md:text-[12px] md:w-full">See Sample Dashboard</h1>
          </a>
        </NuxtLink>
      
      </div>
    </div>
  </header>
</template>