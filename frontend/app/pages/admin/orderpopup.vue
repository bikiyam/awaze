<template>
  <div :class="{ 'light-theme': !isDark }" class="flex flex-col min-h-screen transition-colors duration-300 bg-[#050806] text-white font-sans antialiased selection:bg-emerald-500 selection:text-black">
    
    <header class="sticky top-0 z-40 bg-[#050806]/95 backdrop-blur-md border-b border-emerald-950/40 px-4 py-3.5 md:px-8">
      <div class="max-w-7xl mx-auto flex sm:flex-row items-center justify-between gap-4">
        <button 
          @click="goBackToMenu"
          class="flex items-center space-x-2 text-emerald-400 hover:text-emerald-300 font-bold text-xs md:text-sm transition-all group cursor-pointer"
        >
          <div class="p-2 rounded-lg bg-[#0d1410] border border-emerald-950/80 group-hover:border-emerald-500 transition-colors">
            <svg class="h-4 w-4 transform group-hover:-translate-x-0.5 transition-transform" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.5">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15 19l-7-7 7-7" />
            </svg>
          </div>
          <span class="uppercase tracking-wider font-extrabold">{{ t('backToMenu') }}</span>
        </button>

        <div class="flex items-center space-x-3">
          <div>
            <div class="flex flex-col">
              <div class="flex items-baseline gap-1.5">
                <h1 class="font-black text-lg md:text-2xl tracking-tight text-white uppercase leading-none">{{ t('myOrdersHeader') }}</h1>
              </div>
            </div>
          </div>
        </div>
        <div class="w-24"></div>

      </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-6 md:py-8 md:px-8 w-full">
      
      <div class="flex flex-row gap-4 mb-8 text-center">
        <div class="bg-[#0d1410] border border-emerald-950/60 px-2 py-3 rounded-2xl flex items-center justify-between w-full">
          <div class="w-full">
            <span class=" text-xs font-bold text-zinc-400 uppercase tracking-wider">{{ t('totalOrders') }}</span>
            <h2 class="text-2xl md:text-3xl font-black text-white mt-1">{{ orders.length }}</h2>
          </div>
        </div>

        <div class="bg-[#0d1410] border border-emerald-950/60 px-2 py-3 rounded-2xl flex items-center justify-between w-full">
          <div class="w-full">
            <span class="text-xs font-bold text-zinc-400 uppercase tracking-wider">{{ t('activeOrders') }}</span>
            <h2 class="text-2xl md:text-3xl font-black text-emerald-400 mt-1">{{ activeOrdersCount }}</h2>
          </div>
        </div>

        <div class="bg-[#0d1410] border border-emerald-950/60 px-4 py-3 rounded-2xl flex items-center justify-between w-full">
          <div class="w-full">
            <span class="text-xs font-bold text-zinc-400 uppercase tracking-wider">{{ t('Total Price') }}</span>
            <h2 class="text-xl md:text-2xl font-black text-emerald-400 mt-1">{{ totalSpentAmount.toLocaleString() }} Birr</h2>
          </div>
        </div>
      </div>

      <div class="flex flex-col sm:flex-row items-center justify-between gap-4 mb-6 pb-4 border-b border-emerald-950/40">
        <div class="flex p-0.5 rounded-xl border border-emerald-950 w-full sm:w-auto overflow-none">
          <button 
            @click="orderFilter = 'all'"
            :class="orderFilter === 'all' ? 'bg-emerald-600 text-black shadow-lg font-black' : 'text-zinc-400 hover:text-white font-semibold'"
            class="px-4 py-2 rounded-lg text-xs transition-all whitespace-nowrap cursor-pointer"
          >
            {{ t('allOrdersTab') }} ({{ orders.length }})
          </button>
          <button 
            @click="orderFilter = 'active'"
            :class="orderFilter === 'active' ? 'bg-emerald-600 text-black shadow-lg font-black' : 'text-zinc-400 hover:text-white font-semibold'"
            class="px-4 py-2 rounded-lg text-xs transition-all whitespace-nowrap cursor-pointer"
          >
            {{ t('activeTab') }}
          </button>
          <button 
            @click="orderFilter = 'completed'"
            :class="orderFilter === 'completed' ? 'bg-emerald-600 text-black shadow-lg font-black' : 'text-zinc-400 hover:text-white font-semibold'"
            class="px-4 py-2 rounded-lg text-xs transition-all whitespace-nowrap cursor-pointer"
          >
            {{ t('completedTab') }}
          </button>
        </div>

        <div class="relative w-full sm:w-72">
          <svg class="absolute left-3.5 top-3 h-4 w-4 text-zinc-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <input 
            type="text" 
            v-model="orderSearchQuery" 
            :placeholder="t('searchOrdersPlaceholder')"
            class="w-full bg-[#0d1410] border border-emerald-950/80 text-white placeholder-zinc-500 rounded-lg py-2 pl-10 pr-4 focus:outline-none focus:border-emerald-500 text-xs"
          />
        </div>
      </div>

      <div v-if="filteredOrders.length === 0" class="text-center py-16 bg-[#0d1410] rounded-2xl border border-emerald-950/60 max-w-lg mx-auto px-4">
        <div class="bg-[#101b15] p-4 rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4 border border-emerald-900/40">
          <svg class="h-8 w-8 text-emerald-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
          </svg>
        </div>
        <h3 class="text-lg font-bold text-white mb-2">{{ t('noOrdersTitle') }}</h3>
        <p class="text-zinc-400 text-xs max-w-sm mx-auto mb-6">{{ t('noOrdersDesc') }}</p>
        <button 
          @click="goBackToMenu"
          class="bg-emerald-600 hover:bg-emerald-500 text-black text-xs font-bold px-6 py-2.5 rounded-lg transition cursor-pointer"
        >
          {{ t('browseMenuBtn') }}
        </button>
      </div>

      <div v-else class="space-y-4">
        <div 
          v-for="order in filteredOrders" 
          :key="order.id"
          class="bg-[#0d1410] border border-emerald-950/80 hover:border-emerald-800 rounded-2xl p-5 transition-all shadow-lg items-start md:items-center justify-between gap-4"
        >
          <div class="flex items-start gap-4">
            <div class="w-12 h-12 rounded-xl bg-[#101b15] border border-emerald-900/60 flex items-center justify-center text-emerald-400 font-black text-sm shrink-0">
              <span>#{{ order.id.toString().slice(-3) }}</span>
            </div>
            <div>
              <div class="flex ">
                <h3 class="font-bold text-base text-white">Food Name Food Name Food Name Food Name Food Name </h3>
              </div>
              
              <div class="flex items-center gap-2.5 flex-wrap w-full">
                
                <span 
                  :class="{
                    'bg-amber-950/80 text-amber-400 border-amber-900/50': order.status === 'Preparing',
                    'bg-emerald-950/80 text-emerald-400 border-emerald-900/50': order.status === 'Delivered' || order.status === 'Completed',
                    'bg-blue-950/80 text-blue-400 border-blue-900/50': order.status === 'On the Way',
                    'bg-red-950/80 text-red-400 border-red-900/50': order.status === 'Cancelled'
                  }"
                  class="text-[10px] font-extrabold px-2.5 py-0.5 rounded-full border uppercase tracking-wider"
                >
                  {{ translateStatus(order.status) }}
                </span>
              </div>
              <p class="text-xs text-zinc-400 mt-1">
                {{ order.restaurantName }} • <span class="text-zinc-300 font-medium">{{ order.date }}</span>
              </p>
              
              <div class="flex items-center gap-2 mt-2.5 flex-wrap">
                <span 
                  v-for="(item, idx) in order.items" 
                  :key="idx"
                  class="bg-[#101b15] text-zinc-300 border border-emerald-950 text-[11px] px-2.5 py-1 rounded-md font-medium"
                >
                  {{ item.quantity }}x {{ lang === 'am' ? (item.amharicName || item.name) : item.name }}
                </span>
              </div>
            </div>
          </div>

          <div class="flex items-center justify-between md:justify-end gap-6 w-full md:w-auto pt-3 md:pt-0 border-t md:border-t-0 border-emerald-950/40">
            <div class="text-left md:text-right">
              <span class="text-[10px] text-zinc-500 uppercase font-semibold block">{{ t('totalAmount') }}</span>
              <span class="font-black text-emerald-400 text-lg">{{ order.totalPrice.toLocaleString() }} Birr</span>
            </div>

            <div class="flex items-center gap-2">
              <button 
                @click="viewOrderDetails(order)"
                class="bg-emerald-950/60 hover:bg-emerald-800/80 text-emerald-400 hover:text-emerald-300 text-xs font-bold px-3.5 py-2 rounded-lg border border-emerald-900/60 transition flex items-center space-x-1 cursor-pointer"
              >
                <span>{{ t('viewDetailsBtn') }}</span>
              </button>

              <button 
                v-if="order.status === 'Preparing'"
                @click="cancelOrder(order.id)"
                class="bg-red-950/40 hover:bg-red-900/60 text-red-400 text-xs font-bold px-3 py-2 rounded-lg border border-red-900/50 transition cursor-pointer"
              >
                {{ t('cancelBtn') }}
              </button>

              <button 
                @click="deleteOrder(order.id)"
                class="bg-[#101b15] hover:bg-red-950/60 text-zinc-400 hover:text-red-400 text-xs font-bold p-2 rounded-lg border border-emerald-950 hover:border-red-900 transition cursor-pointer"
                :title="t('deleteOrderTooltip')"
              >
                <svg class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>

    </main>

    <div 
      v-if="selectedOrder"
      @click.self="selectedOrder = null"
      class="fixed inset-0 z-50 bg-black/90 backdrop-blur-md flex items-center justify-center p-4 overflow-y-auto"
    >
      <div 
        @click.stop
        class="bg-[#0d1410] border border-emerald-800/60 w-full max-w-xl rounded-2xl overflow-hidden shadow-2xl relative flex flex-col my-auto"
      >
        <div class="p-5 border-b border-emerald-950/80 flex items-center justify-between bg-[#080f0c]">
          <div>
            <h3 class="font-black text-lg text-white">{{ t('orderDetailsTitle') }} #{{ selectedOrder.id }}</h3>
            <p class="text- text-emerald-400 font-semibold">{{ selectedOrder.restaurantName }} • {{ selectedOrder.date }}</p>
          </div>
          <button 
            @click="selectedOrder = null"
            class="bg-black/75 hover:bg-black p-2 rounded-full text-white border border-emerald-900/60 transition cursor-pointer"
          >
            <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>

        <div class="p-5 overflow-y-auto space-y-4 max-h-[60vh] scrollbar-thin">
          <div class="flex items-center justify-between bg-[#101b15] p-3 rounded-xl border border-emerald-950">
            <span class="text-xs text-zinc-400 uppercase font-bold">{{ t('orderStatusLabel') }}</span>
            <span class="text-xs font-black uppercase text-emerald-400 px-3 py-1 rounded-md bg-emerald-950/80 border border-emerald-900">
              {{ translateStatus(selectedOrder.status) }}
            </span>
          </div>

          <div>
            <h4 class="text-xs font-extrabold uppercase tracking-wider text-emerald-400 mb-3">{{ t('orderedItemsHeader') }}</h4>
            <div class="space-y-2.5">
              <div 
                v-for="(item, idx) in selectedOrder.items" 
                :key="idx"
                class="flex items-center justify-between bg-[#050806] p-3 rounded-xl border border-emerald-950"
              >
                <div class="flex items-center gap-3">
                  <div class="w-10 h-10 rounded-lg overflow-hidden bg-zinc-900 shrink-0">
                    <img :src="item.image" :alt="item.name" class="w-full h-full object-cover" />
                  </div>
                  <div>
                    <h5 class="font-bold text-xs text-white">{{ lang === 'am' ? (item.amharicName || item.name) : item.name }}</h5>
                    <span class="text-[10px] text-zinc-400">{{ t('qty') }}: {{ item.quantity }} × {{ item.price.toLocaleString() }} Birr</span>
                  </div>
                </div>
                <span class="font-black text-emerald-400 text-xs">{{ (item.price * item.quantity).toLocaleString() }} Birr</span>
              </div>
            </div>
          </div>

          <div class="bg-[#101b15] p-4 rounded-xl border border-emerald-950 space-y-2">
            <div class="flex justify-between text-xs text-zinc-400">
              <span>{{ t('subtotal') }}</span>
              <span>{{ selectedOrder.totalPrice.toLocaleString() }} Birr</span>
            </div>
            <div class="flex justify-between text-xs text-zinc-400">
              <span>{{ t('serviceFee') }}</span>
              <span>0 Birr</span>
            </div>
            <div class="border-t border-emerald-950 pt-2 flex justify-between text-sm font-black text-white">
              <span>{{ t('totalAmount') }}</span>
              <span class="text-emerald-400">{{ selectedOrder.totalPrice.toLocaleString() }} Birr</span>
            </div>
          </div>
        </div>

        <div class="p-4 bg-[#080f0c] border-t border-emerald-950/80 flex items-center justify-between">
          <button 
            v-if="selectedOrder.status === 'Preparing'"
            @click="cancelOrder(selectedOrder.id); selectedOrder = null;"
            class="bg-red-950/60 hover:bg-red-900/80 text-red-400 text-xs font-extrabold px-4 py-2.5 rounded-lg border border-red-900 transition cursor-pointer"
          >
            {{ t('cancelOrderBtn') }}
          </button>
          <div v-else></div>

          <button 
            @click="selectedOrder = null"
            class="bg-emerald-600 hover:bg-emerald-500 text-black text-xs font-extrabold px-6 py-2.5 rounded-lg transition cursor-pointer"
          >
            {{ t('closeCard') }}
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const isDark = ref(true)
const lang = ref('en')
const orderFilter = ref('all')
const orderSearchQuery = ref('')
const selectedOrder = ref(null)

const orders = ref([
  {
    id: 'AWZ-9482',
    date: 'Today, 2:45 PM',
    restaurantName: 'Girum Burger & Pizza',
    status: 'Preparing',
    totalPrice: 1400,
    items: [
      { name: 'Special Sizzling Tibs', amharicName: 'ልዩ የጋለ ጥብስ', quantity: 2, price: 680, image: 'https://images.unsplash.com/photo-1544025162-d76694265947?auto=format&fit=crop&q=80&w=600' },
      { name: 'Habesha Cold Lager', amharicName: 'ሐበሻ የቀዘቀዘ ቢራ', quantity: 1, price: 120, image: 'https://images.unsplash.com/photo-1500217052183-bc01eee1a74e?q=80&w=688&auto=format&fit=crop' }
    ]
  },
])

const i18n = {
  en: {
    backToMenu: "Back to Menu",
    myOrdersHeader: "My Orders",
    totalOrders: "Total Orders",
    activeOrders: "Active Orders",
    totalSpent: "Total Spent",
    allOrdersTab: "All Orders",
    activeTab: "Active",
    completedTab: "Completed",
    searchOrdersPlaceholder: "Search by Food Name or Order ID...",
    noOrdersTitle: "No orders found",
    noOrdersDesc: "You haven't placed any orders matching your search or filter criteria.",
    browseMenuBtn: "Browse Menu",
    totalAmount: "Total Amount",
    viewDetailsBtn: "Details",
    cancelBtn: "Cancel",
    cancelOrderTooltip: "Cancel order",
    deleteOrderTooltip: "Delete order record",
    orderDetailsTitle: "Order Details",
    orderStatusLabel: "Status",
    orderedItemsHeader: "Ordered Items",
    qty: "Qty",
    subtotal: "Subtotal",
    serviceFee: "Service & Taxes",
    cancelOrderBtn: "Cancel Order",
    closeCard: "Close",
    digitalMenu: "AWAZE DIGITAL MENU",
    rights: "© 2026 Awaze Menu. All rights reserved."
  },
}

const activeOrdersCount = computed(() => {
  return orders.value.filter(o => o.status === 'Preparing' || o.status === 'On the Way').length
})

const totalSpentAmount = computed(() => {
  return orders.value
    .filter(o => o.status !== 'Cancelled')
    .reduce((sum, o) => sum + o.totalPrice, 0)
})

const filteredOrders = computed(() => {
  let list = orders.value
  if (orderFilter.value === 'active') {
    list = list.filter(o => o.status === 'Preparing' || o.status === 'On the Way')
  } else if (orderFilter.value === 'completed') {
    list = list.filter(o => o.status === 'Delivered' || o.status === 'Completed')
  }

  if (orderSearchQuery.value && orderSearchQuery.value.trim() !== '') {
    const q = orderSearchQuery.value.toLowerCase()
    list = list.filter(o => 
      o.id.toLowerCase().includes(q) || 
      o.restaurantName.toLowerCase().includes(q)
    )
  }
  return list
})

function toggleTheme() {
  isDark.value = !isDark.value
}

function toggleLang() {
  lang.value = lang.value === 'en' ? 'am' : 'en'
}

function t(key) {
  return (i18n[lang.value] && i18n[lang.value][key]) ? i18n[lang.value][key] : (i18n['en'][key] || key)
}

function translateStatus(status) {
  if (lang.value === 'am') {
    if (status === 'Preparing') return 'በዝግጅት ላይ'
    if (status === 'Delivered' || status === 'Completed') return 'ተጠናቋል'
    if (status === 'On the Way') return 'በመንገድ ላይ'
    if (status === 'Cancelled') return 'ተሰርዟል'
  }
  return status
}

function goBackToMenu() {
  // In Nuxt / Vue router, navigate back or to index
  if (window.history.length > 1) {
    window.history.back()
  } else {
    window.location.href = '/'
  }
}

function viewOrderDetails(order) {
  selectedOrder.value = order
}

function cancelOrder(id) {
  const order = orders.value.find(o => o.id === id)
  if (order) {
    order.status = 'Cancelled'
  }
}

function deleteOrder(id) {
  orders.value = orders.value.filter(o => o.id !== id)
  if (selectedOrder.value && selectedOrder.value.id === id) {
    selectedOrder.value = null
  }
}
</script>

<style scoped>
body, h1, h2, h3, h4, h5, h6, span, p, button, input, textarea, a {
  font-family: 'Literata', serif !important;
}

.scrollbar-thin::-webkit-scrollbar {
  height: 6px;
  width: 6px;
}
.scrollbar-thin::-webkit-scrollbar-track {
  background: transparent;
}
.scrollbar-thin::-webkit-scrollbar-thumb {
  background: #064e3b;
  border-radius: 5px;
}
.scrollbar-thin::-webkit-scrollbar-thumb:hover {
  background: #10b981;
}

.light-theme {
  background-color: #f8fafc !important;
  color: #0f172a !important;
}

.light-theme header {
  background-color: rgba(255, 255, 255, 0.95) !important;
  border-color: #e2e8f0 !important;
}

.light-theme input {
  background-color: #ffffff !important;
  border-color: #cbd5e1 !important;
  color: #0f172a !important;
}

.light-theme input::placeholder {
  color: #64748b !important;
}

.light-theme button[aria-label="Toggle Theme"] {
  background-color: #ffffff !important;
  border-color: #cbd5e1 !important;
  color: #059669 !important;
}

.light-theme .bg-\[\#0d1410\] {
  background-color: #ffffff !important;
  border-color: #e2e8f0 !important;
}

.light-theme .bg-\[\#050806\] {
  background-color: #f1f5f9 !important;
}

.light-theme .bg-\[\#101b15\] {
  background-color: #f0fdf4 !important;
  border-color: #d1fae5 !important;
}

.light-theme .bg-\[\#030504\],
.light-theme .bg-\[\#080f0c\] {
  background-color: #ffffff !important;
  border-color: #e2e8f0 !important;
}

.light-theme .text-white {
  color: #0f172a !important;
}

.light-theme .text-zinc-300 {
  color: #334155 !important;
}

.light-theme .text-zinc-400 {
  color: #475569 !important;
}

.light-theme .text-emerald-400 {
  color: #059669 !important;
}

.light-theme footer {
  background-color: #ffffff !important;
  border-color: #e2e8f0 !important;
}
</style>