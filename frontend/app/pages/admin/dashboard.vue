<template>
<div class="h-full font-sans text-white overflow-hidden">
    <div id="app" v-cloak class="flex h-full w-full bg-slate-100 overflow-hidden">

        <!-- Sidebar Navigation (Dark themed as per reference) -->
        <aside class="fixed h-screen w-20 lg:w-64 bg-[#121824] text-slate-300 flex flex-col justify-between flex-shrink-0 transition-all duration-300 shadow-xl z-20">
            <div>
                <!-- Logo Header -->
                <div class="h-20 flex items-center px-4 lg:px-6 gap-3 border-b border-slate-800">
                    <div class="w-10 h-10 rounded-xl bg-emerald-500 flex items-center justify-center text-white font-bold text-xl shadow-lg shadow-emerald-500/30">
                        A
                    </div>
                    <div class="hidden lg:block overflow-hidden">
                        <h1 class="text-white font-bold text-base truncate">AWAZE</h1>
                        <span class="text-xs text-emerald-400 font-medium">Admin Dashboard</span>
                    </div>
                </div>

                <!-- Navigation Links -->
                <nav class="p-3 space-y-2 mt-4">
                    <button @click="currentTab = 'status'" :class="currentTab === 'status' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm">
                        <i class="fa-solid fa-chart-pie text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Status (Dashboard)</span>
                    </button>
                    <button @click="currentTab = 'items'" :class="currentTab === 'items' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm">
                        <i class="fa-solid fa-utensils text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Items / Menu</span>
                    </button>
                    <button @click="currentTab = 'notification'" :class="currentTab === 'notification' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm relative">
                        <i class="fa-solid fa-bell text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Notification</span>
                        <span v-if="unreadCount > 0" class="absolute right-3 lg:right-4 top-3 px-1.5 py-0.5 bg-rose-500 text-white text-[10px] rounded-full font-bold">{{ unreadCount }}</span>
                    </button>
                    <button @click="currentTab = 'analytics'" :class="currentTab === 'analytics' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm">
                        <i class="fa-solid fa-chart-line text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Analytics</span>
                    </button>
                    <button @click="currentTab = 'subscription'" :class="currentTab === 'subscription' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm">
                        <i class="fa-solid fa-wallet text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Subscription</span>
                    </button>
                    <button @click="currentTab = 'settings'" :class="currentTab === 'settings' ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-600/30' : 'hover:bg-slate-800 text-slate-400 hover:text-white'" class="w-full flex items-center gap-4 px-3.5 py-3 rounded-xl transition-all font-medium text-sm">
                        <i class="fa-solid fa-gear text-lg w-5 text-center"></i>
                        <span class="hidden lg:block">Settings</span>
                    </button>
                </nav>
            </div>

            <!-- Bottom User Mini Profile / Logout -->
            <div class="p-4 border-t border-slate-800 flex items-center justify-between">
                <div class="hidden lg:flex items-center gap-3">
                    <img :src="restaurant.image || 'https://placehold.co/100x100/10b981/ffffff?text=Res'" class="w-9 h-9 rounded-full object-cover border border-slate-700" alt="Avatar">
                    <div class="overflow-hidden">
                        <p class="text-xs font-bold text-white truncate">{{ restaurant.name }}</p>
                        <p class="text-[10px] text-slate-400 truncate">{{ restaurant.phone }}</p>
                    </div>
                </div>
                <button @click="logout" title="Log Out" class="p-2 text-slate-400 hover:text-rose-400 hover:bg-slate-800 rounded-lg transition-all mx-auto lg:mx-0">
                    <i class="fa-solid fa-right-from-bracket text-lg"></i>
                </button>
            </div>
        </aside>

        <!-- Main Content Area -->
        <main class="flex-1 flex flex-col h-full overflow-hidden pl-20 lg:pl-64">
            <!-- Top Navbar matching Frame 7 reference -->
            <header class="fixed h-20 bg-white border-b border-slate-200 px-6 flex items-center justify-between flex-shrink-0 shadow-sm z-10 w-full lg:pr-72">
                <div class="flex items-center gap-4 w-1/3">
                    <h2 class="text-xl font-bold text-slate-800 capitalize">{{ currentTabTitle }}</h2>
                </div>

                <!-- Global Search & User info -->
                <div class="flex items-center gap-4">
                    <div class="relative hidden md:block w-64">
                        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-sm"></i>
                        <input v-model="searchQuery" type="text" placeholder="Search menu, orders, settings..." class="w-full pl-10 pr-4 py-2 bg-slate-100 border-none rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500/50">
                    </div>

                    <!-- Notification bell shortcut -->
                    <button @click="currentTab = 'notification'" class="relative p-2.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-600 transition-all">
                        <i class="fa-regular fa-bell text-lg"></i>
                        <span v-if="unreadCount > 0" class="absolute -top-1 -right-1 w-5 h-5 bg-rose-500 text-white text-xs rounded-full flex items-center justify-center font-bold shadow">{{ unreadCount }}</span>
                    </button>

                    <!-- Profile Avatar pill -->
                    <div class="flex items-center gap-3 pl-2 border-l border-slate-200">
                        <img :src="restaurant.image || 'https://placehold.co/100x100/10b981/ffffff?text=Res'" class="w-10 h-10 rounded-xl object-cover shadow" alt="Admin">
                        <div class="hidden sm:block text-left">
                            <h4 class="text-sm font-bold text-slate-800 leading-tight">{{ restaurant.name }}</h4>
                            <span class="text-xs text-slate-500 font-medium">Manager / Admin</span>
                        </div>
                    </div>
                </div>
            </header>

            <!-- Scrollable Viewport Container -->
            <div class="flex-1 overflow-y-auto p-6 lg:p-8 mt-20">

                <!-- TOAST / MESSAGE BOX -->
                <div v-if="toast.show" :class="toast.type === 'error' ? 'bg-rose-500 text-white' : 'bg-emerald-600 text-white'" class="fixed bottom-6 right-6 px-5 py-3 rounded-xl shadow-2xl flex items-center gap-3 z-50 transition-all animate-bounce">
                    <i :class="toast.type === 'error' ? 'fa-solid fa-circle-exclamation' : 'fa-solid fa-circle-check'" class="text-lg"></i>
                    <span class="text-sm font-semibold">{{ toast.message }}</span>
                </div>

                <!-- ================= SUB-PAGE 1: STATUS (DASHBOARD) ================= -->
                <div v-if="currentTab === 'status'" class="space-y-6">
                    <!-- Top Metrics Cards (Sales, Orders, Customers, Tables, Rooms) -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-5">
                        <!-- Total Sales Card -->
                        <div class="bg-gradient-to-br from-purple-500 to-indigo-600 text-white p-6 rounded-2xl shadow-lg relative overflow-hidden">
                            <div class="absolute right-4 top-4 w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                                <i class="fa-solid fa-wallet text-xl"></i>
                            </div>
                            <p class="text-purple-100 text-xs font-semibold uppercase tracking-wider">Total Views</p>
                            <h3 class="text-3xl font-extrabold mt-2">${{ totalSales.toLocaleString() }}</h3>
                            <div class="mt-4 flex items-center gap-1.5 text-xs bg-white/20 px-2.5 py-1 rounded-lg w-max backdrop-blur-sm">
                                <i class="fa-solid fa-arrow-trend-up"></i>
                                <span>+12% from yesterday</span>
                            </div>
                        </div>

                        <!-- Total Orders Card -->
                        <div class="bg-gradient-to-br from-amber-400 to-orange-500 text-white p-6 rounded-2xl shadow-lg relative overflow-hidden">
                            <div class="absolute right-4 top-4 w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                                <i class="fa-solid fa-cart-shopping text-xl"></i>
                            </div>
                            <p class="text-amber-100 text-xs font-semibold uppercase tracking-wider">Total Orders</p>
                            <h3 class="text-3xl font-extrabold mt-2">{{ totalOrders }}</h3>
                            <div class="mt-4 flex items-center gap-1.5 text-xs bg-white/20 px-2.5 py-1 rounded-lg w-max backdrop-blur-sm">
                                <i class="fa-solid fa-arrow-trend-up"></i>
                                <span>+8% from yesterday</span>
                            </div>
                        </div>

                        <!-- Menu Stats Card -->
                        <div class="bg-gradient-to-br from-emerald-500 to-teal-600 text-white p-6 rounded-2xl shadow-lg relative overflow-hidden">
                            <div class="absolute right-4 top-4 w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                                <i class="fa-solid fa-utensils text-xl"></i>
                            </div>
                            <p class="text-emerald-100 text-xs font-semibold uppercase tracking-wider">Foods & Drinks / Categories</p>
                            <h3 class="text-3xl font-extrabold mt-2">{{ foods.length }} <span class="text-lg font-normal">/ {{ categories.length }}</span></h3>
                            <div class="mt-4 flex items-center gap-1.5 text-xs bg-white/20 px-2.5 py-1 rounded-lg w-max backdrop-blur-sm">
                                <i class="fa-solid fa-tag"></i>
                                <span>Avg Price: ${{ avgFoodPrice }}</span>
                            </div>
                        </div>

                        <!-- Tables Card -->
                        <div v-if="settings.orderingOption" class="bg-gradient-to-br from-blue-500 to-cyan-600 text-white p-6 rounded-2xl shadow-lg relative overflow-hidden">
                            <div class="absolute right-4 top-4 w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                                <i class="fa-solid fa-chair text-xl"></i>
                            </div>
                            <p class="text-blue-100 text-xs font-semibold uppercase tracking-wider">Active Tables</p>
                            <h3 class="text-3xl font-extrabold mt-2">{{ tablesCount }}</h3>
                            <div class="mt-4 flex items-center gap-1.5 text-xs bg-white/20 px-2.5 py-1 rounded-lg w-max backdrop-blur-sm">
                                <i class="fa-solid fa-qrcode"></i>
                                <span>QR Enabled</span>
                            </div>
                        </div>

                        <!-- Rooms Card -->
                        <div v-if="settings.roomIntegration" class="bg-gradient-to-br from-rose-500 to-pink-600 text-white p-6 rounded-2xl shadow-lg relative overflow-hidden">
                            <div class="absolute right-4 top-4 w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                                <i class="fa-solid fa-hotel text-xl"></i>
                            </div>
                            <p class="text-rose-100 text-xs font-semibold uppercase tracking-wider">Hotel Rooms</p>
                            <h3 class="text-3xl font-extrabold mt-2">{{ roomsCount }}</h3>
                            <div class="mt-4 flex items-center gap-1.5 text-xs bg-white/20 px-2.5 py-1 rounded-lg w-max backdrop-blur-sm">
                                <i class="fa-solid fa-door-open"></i>
                                <span>Room Service QR</span>
                            </div>
                        </div>
                    </div>

                    <!-- Quick Overview Grids (Charts & Tables) -->
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                        <!-- Left 2 cols: Customer Satisfaction / Revenue Trend -->
                        <div class="lg:col-span-2 bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <div class="flex items-center justify-between mb-4">
                                <h3 class="font-bold text-slate-800 text-lg">Customer Satisfaction & Sales Flow</h3>
                                <span class="text-xs font-medium text-emerald-600 bg-emerald-50 px-3 py-1 rounded-full">Live Analytics</span>
                            </div>
                            <div class="h-72 relative">
                                <canvas id="statusChart"></canvas>
                            </div>
                        </div>

                        <!-- Right 1 col: Branch & Status info -->
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 flex flex-col justify-between">
                            <div>
                                <h3 class="font-bold text-slate-800 text-lg mb-2">Restaurant Profile</h3>
                                <p class="text-xs text-slate-500 mb-4">{{ restaurant.description }}</p>
                                <div class="space-y-3">
                                    <div class="flex items-center justify-between text-sm py-2 border-b border-slate-100">
                                        <span class="text-slate-500">Location</span>
                                        <span class="font-semibold text-slate-800">{{ restaurant.location }}</span>
                                    </div>
                                    <div class="flex items-center justify-between text-sm py-2 border-b border-slate-100">
                                        <span class="text-slate-500">Phone</span>
                                        <span class="font-semibold text-slate-800">{{ restaurant.phone }}</span>
                                    </div>
                                    <div class="flex items-center justify-between text-sm py-2 border-b border-slate-100">
                                        <span class="text-slate-500">Branch Status</span>
                                        <span class="font-semibold text-emerald-600">{{ settings.restaurantBranch ? 'Multi-Branch Active' : 'Single Location' }}</span>
                                    </div>
                                </div>
                            </div>
                            <button @click="currentTab = 'items'" class="w-full mt-6 py-3 bg-slate-900 hover:bg-slate-800 text-white rounded-xl font-semibold text-sm transition-all shadow-md">
                                Manage Menu Items <i class="fa-solid fa-arrow-right ml-2"></i>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- ================= SUB-PAGE 2: ITEMS (MENU MANAGEMENT) ================= -->
                <div v-if="currentTab === 'items'" class="space-y-6">
                    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">Menu Catalog (Foods & Drinks)</h3>
                            <p class="text-xs text-slate-500 mt-1">Add, edit, view details, or delete items instantly.</p>
                        </div>
                        <div class="flex items-center gap-3">
                            <select v-model="selectedCategoryFilter" class="px-4 py-2 bg-slate-100 border-none rounded-xl text-sm font-medium focus:outline-none focus:ring-2 focus:ring-emerald-500">
                                <option value="All">All Categories</option>
                                <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
                            </select>
                            <button @click="openAddFoodModal" class="px-4 py-2.5 bg-emerald-600 hover:bg-emerald-700 text-white font-semibold text-sm rounded-xl shadow-lg shadow-emerald-600/30 transition-all flex items-center gap-2">
                                <i class="fa-solid fa-plus"></i> Add New Item
                            </button>
                        </div>
                    </div>

                    <!-- Foods Grid -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div v-for="food in filteredFoods" :key="food.id" class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden flex flex-col justify-between hover:shadow-md transition-all group">
                            <div>
                                <div class="relative h-48 overflow-hidden bg-slate-100">
                                    <img :src="food.image" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500" alt="Food">
                                    <span class="absolute top-3 left-3 px-3 py-1 bg-black/60 backdrop-blur-md text-white text-xs font-semibold rounded-lg">{{ food.category }}</span>
                                    <span class="absolute top-3 right-3 px-3 py-1 bg-emerald-600 text-white text-xs font-bold rounded-lg shadow">${{ food.price }}</span>
                                </div>
                                <div class="p-5">
                                    <h4 class="font-bold text-slate-800 text-lg truncate">{{ food.name }}</h4>
                                    <p class="text-xs text-slate-500 mt-1 line-clamp-2">{{ food.description }}</p>
                                </div>
                            </div>
                            <div class="p-5 pt-0 flex items-center justify-between border-t border-slate-100 mt-4">
                                <button @click="viewFoodDetails(food)" class="text-xs font-semibold text-emerald-600 hover:text-emerald-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-eye"></i> View & Edit
                                </button>
                                <div class="flex items-center gap-2">
                                    <button @click="editFood(food)" class="p-2 text-slate-400 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition-all" title="Edit">
                                        <i class="fa-solid fa-pen text-sm"></i>
                                    </button>
                                    <button @click="deleteFood(food.id)" class="p-2 text-slate-400 hover:text-rose-600 hover:bg-rose-50 rounded-lg transition-all" title="Delete">
                                        <i class="fa-solid fa-trash text-sm"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- ================= SUB-PAGE 3: NOTIFICATION (ACTIVITY & COMMENTS) ================= -->
                <div v-if="currentTab === 'notification'" class="space-y-6">
                    <div class="flex items-center justify-between bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">Customer Activity & Feedback</h3>
                            <p class="text-xs text-slate-500 mt-1">Real-time comments, ratings, and customer reviews on your menu.</p>
                        </div>
                        <button @click="markAllNotificationsRead" class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold text-xs rounded-xl transition-all">
                            Mark All as Read
                        </button>
                    </div>

                    <div class="space-y-4">
                        <div v-for="notif in notifications" :key="notif.id" :class="notif.unread ? 'border-l-4 border-emerald-500 bg-emerald-50/40' : 'bg-white'" class="p-5 rounded-2xl shadow-sm border border-slate-200 flex flex-col sm:flex-row sm:items-center justify-between gap-4 transition-all">
                            <div class="flex items-start gap-4">
                                <div class="w-12 h-12 rounded-xl bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold text-lg flex-shrink-0">
                                    <i class="fa-solid fa-comment-dots"></i>
                                </div>
                                <div>
                                    <div class="flex items-center gap-2">
                                        <h4 class="font-bold text-slate-800 text-sm">{{ notif.author }}</h4>
                                        <span class="text-xs text-slate-400">• {{ notif.time }}</span>
                                    </div>
                                    <p class="text-xs text-emerald-700 font-medium mt-0.5">Commented on: <strong class="underline">{{ notif.foodName }}</strong></p>
                                    <p class="text-sm text-slate-600 mt-2 italic bg-white p-3 rounded-xl border border-slate-200">"{{ notif.comment }}"</p>
                                </div>
                            </div>
                            <div class="flex items-center gap-3 self-end sm:self-center">
                                <div class="flex items-center gap-1 bg-amber-50 text-amber-600 px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200">
                                    <i class="fa-solid fa-star"></i>
                                    <span>{{ notif.rating }} / 5.0</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- ================= SUB-PAGE 4: SETTINGS (IDENTITY & TOGGLES) ================= -->
                <div v-if="currentTab === 'settings'" class="space-y-6">
                    <div class="bg-white p-6 lg:p-8 rounded-2xl shadow-sm border border-slate-200 space-y-6">
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">Restaurant Identity & Operational Settings</h3>
                            <p class="text-xs text-slate-500 mt-1">Update your restaurant information and configure module toggles below.</p>
                        </div>

                        <!-- Identity Form -->
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 pt-4 border-t border-slate-100">
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-2">Restaurant Name</label>
                                <input v-model="restaurant.name" type="text" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-2">Location / Address</label>
                                <input v-model="restaurant.location" type="text" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-2">Phone Number</label>
                                <input v-model="restaurant.phone" type="text" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-2">Cover Image URL</label>
                                <input v-model="restaurant.image" type="text" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            </div>
                            <div class="md:col-span-2">
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-2">Description</label>
                                <textarea v-model="restaurant.description" rows="3" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500"></textarea>
                            </div>
                        </div>

                        <!-- 3 Required Feature Toggles -->
                        <div class="pt-6 border-t border-slate-100 space-y-4">
                            <h4 class="font-bold text-slate-800 text-base">Advanced Feature Toggles</h4>
                            
                            <!-- Toggle 1: Ordering Option -->
                            <div class="flex items-center justify-between p-4 bg-slate-50 rounded-2xl border border-slate-200">
                                <div>
                                    <h5 class="font-bold text-slate-800 text-sm">Ordering Option (Multiple Tables & QR Codes)</h5>
                                    <p class="text-xs text-slate-500">Enable table-specific QR code ordering for dine-in customers.</p>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" v-model="settings.orderingOption" class="sr-only peer">
                                    <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-emerald-600"></div>
                                </label>
                            </div>

                            <!-- Toggle 2: Room Integration -->
                            <div class="flex items-center justify-between p-4 bg-slate-50 rounded-2xl border border-slate-200">
                                <div>
                                    <h5 class="font-bold text-slate-800 text-sm">Room Integration (Hotel Rooms & QR Codes)</h5>
                                    <p class="text-xs text-slate-500">Enable room service ordering with dedicated room QR codes.</p>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" v-model="settings.roomIntegration" class="sr-only peer">
                                    <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-emerald-600"></div>
                                </label>
                            </div>

                            <!-- Toggle 3: Restaurant Branch -->
                            <div class="flex items-center justify-between p-4 bg-slate-50 rounded-2xl border border-slate-200">
                                <div>
                                    <h5 class="font-bold text-slate-800 text-sm">Restaurant Branch Management</h5>
                                    <p class="text-xs text-slate-500">Manage multiple physical locations and regional menus under one account.</p>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" v-model="settings.restaurantBranch" class="sr-only peer">
                                    <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-emerald-600"></div>
                                </label>
                            </div>
                        </div>

                        <!-- Save Settings Button -->
                        <div class="flex justify-end pt-4">
                            <button @click="saveSettings" class="px-6 py-3 bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm rounded-xl shadow-lg shadow-emerald-600/30 transition-all">
                                Save All Settings
                            </button>
                        </div>
                    </div>
                </div>

                <!-- ================= SUB-PAGE 5: ANALYTICS ================= -->
                <div v-if="currentTab === 'analytics'" class="space-y-6">
                    <!-- Top Metric Cards for Analytics -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <p class="text-xs font-bold text-slate-400 uppercase">QR Scans / Page Views</p>
                            <h3 class="text-3xl font-extrabold text-slate-800 mt-2">24,850</h3>
                            <span class="text-xs text-emerald-600 font-semibold bg-emerald-50 px-2 py-0.5 rounded mt-2 inline-block">+18% this week</span>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <p class="text-xs font-bold text-slate-400 uppercase">Total Orders</p>
                            <h3 class="text-3xl font-extrabold text-slate-800 mt-2">3,420</h3>
                            <span class="text-xs text-emerald-600 font-semibold bg-emerald-50 px-2 py-0.5 rounded mt-2 inline-block">+6% this week</span>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <p class="text-xs font-bold text-slate-400 uppercase">Avg Views / Day</p>
                            <h3 class="text-3xl font-extrabold text-slate-800 mt-2">845</h3>
                            <span class="text-xs text-blue-600 font-semibold bg-blue-50 px-2 py-0.5 rounded mt-2 inline-block">Consistent traffic</span>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <p class="text-xs font-bold text-slate-400 uppercase">Avg Orders / Day</p>
                            <h3 class="text-3xl font-extrabold text-slate-800 mt-2">114</h3>
                            <span class="text-xs text-purple-600 font-semibold bg-purple-50 px-2 py-0.5 rounded mt-2 inline-block">High conversion</span>
                        </div>
                    </div>

                    <!-- Charts Grid -->
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <h3 class="font-bold text-slate-800 text-lg mb-4">Daily / Weekly Visitors</h3>
                            <div class="h-64 relative">
                                <canvas id="visitorChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <h3 class="font-bold text-slate-800 text-lg mb-4">Peak Working Times (Hourly Traffic)</h3>
                            <div class="h-64 relative">
                                <canvas id="peakChart"></canvas>
                            </div>
                        </div>
                    </div>

                    <!-- Top 10 Most Viewed Foods Table -->
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                        <h3 class="font-bold text-slate-800 text-lg mb-4">Top 10 Most Viewed Foods & Drinks</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                               <thead>
                                    <tr class="border-b border-slate-200 text-xs text-slate-400 uppercase">
                                        <th class="py-3 px-4">Rank</th>
                                        <th class="py-3 px-4">Item Name</th>
                                        <th class="py-3 px-4">Views</th>
                                        <th class="py-3 px-4">Orders</th>
                                        <th class="py-3 px-4">Avg Viewing Time</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-slate-100 text-sm">
                                    <tr v-for="(item, index) in topViewedFoods" :key="item.id" class="hover:bg-slate-50 transition-all">
                                        <td class="py-3.5 px-4 font-bold text-slate-500">#{{ index + 1 }}</td>
                                        <td class="py-3.5 px-4 font-bold text-slate-800 flex items-center gap-3">
                                            <img :src="item.image" class="w-8 h-8 rounded-lg object-cover" alt="food">
                                            <span>{{ item.name }}</span>
                                        </td>
                                        <td class="py-3.5 px-4 text-slate-600 font-semibold">{{ item.views.toLocaleString() }}</td>
                                        <td class="py-3.5 px-4 text-emerald-600 font-semibold">{{ item.orders }}</td>
                                        <td class="py-3.5 px-4 text-slate-500">{{ item.avgTime }}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- ================= SUB-PAGE 6: SUBSCRIPTION ================= -->
                <div v-if="currentTab === 'subscription'" class="space-y-6">
                    <div class="bg-gradient-to-br from-slate-900 to-slate-800 text-white p-8 rounded-2xl shadow-xl flex flex-col md:flex-row md:items-center justify-between gap-6">
                        <div>
                            <span class="px-3 py-1 bg-emerald-500 text-white text-xs font-bold rounded-full uppercase tracking-wider">Current Plan</span>
                            <h3 class="text-3xl font-extrabold mt-3">{{ subscription.currentPlan }}</h3>
                            <p class="text-slate-400 text-sm mt-1">Billed monthly • Next payment due on <strong>{{ subscription.nextPayment }}</strong></p>
                        </div>
                        <div class="text-right">
                            <p class="text-3xl font-black text-emerald-400">${{ subscription.price }}<span class="text-sm font-normal text-slate-400">/mo</span></p>
                            <span class="inline-block mt-2 px-3 py-1 bg-emerald-500/20 text-emerald-400 text-xs font-bold rounded-lg border border-emerald-500/30">Payment Status: {{ subscription.status }}</span>
                        </div>
                    </div>

                    <!-- Upgrade / Downgrade Plans -->
                    <div class="bg-white p-6 lg:p-8 rounded-2xl shadow-sm border border-slate-200">
                        <h3 class="text-xl font-bold text-slate-800 mb-6">Available Plans & Upgrade Options</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                            <div v-for="plan in subscription.plans" :key="plan.name" :class="subscription.currentPlan === plan.name ? 'border-2 border-emerald-500 bg-emerald-50/20' : 'border border-slate-200'" class="p-6 rounded-2xl flex flex-col justify-between transition-all">
                                <div>
                                    <h4 class="font-bold text-lg text-slate-800">{{ plan.name }}</h4>
                                    <p class="text-2xl font-black text-emerald-600 mt-2">${{ plan.price }}<span class="text-xs text-slate-400 font-normal">/mo</span></p>
                                    <p class="text-xs text-slate-500 mt-3">{{ plan.desc }}</p>
                                    <ul class="mt-4 space-y-2 text-xs text-slate-600">
                                        <li v-for="feat in plan.features" :key="feat" class="flex items-center gap-2">
                                            <i class="fa-solid fa-check text-emerald-500"></i> {{ feat }}
                                        </li>
                                    </ul>
                                </div>
                                <button @click="changePlan(plan.name)" :disabled="subscription.currentPlan === plan.name" class="mt-6 w-full py-2.5 rounded-xl text-sm font-semibold transition-all" :class="subscription.currentPlan === plan.name ? 'bg-slate-200 text-slate-500 cursor-not-allowed' : 'bg-emerald-600 hover:bg-emerald-700 text-white shadow-md'">
                                    {{ subscription.currentPlan === plan.name ? 'Current Plan' : 'Upgrade Plan' }}
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- Payment History -->
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                        <h3 class="font-bold text-slate-800 text-lg mb-4">Payment History</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead>
                                    <tr class="border-b border-slate-200 text-xs text-slate-400 uppercase">
                                        <th class="py-3 px-4">Invoice ID</th>
                                        <th class="py-3 px-4">Date</th>
                                        <th class="py-3 px-4">Plan</th>
                                        <th class="py-3 px-4">Amount</th>
                                        <th class="py-3 px-4">Status</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-slate-100 text-sm">
                                    <tr v-for="hist in subscription.history" :key="hist.id" class="hover:bg-slate-50 transition-all">
                                        <td class="py-3.5 px-4 font-bold text-slate-800">{{ hist.id }}</td>
                                        <td class="py-3.5 px-4 text-slate-500">{{ hist.date }}</td>
                                        <td class="py-3.5 px-4 text-slate-700 font-medium">{{ hist.plan }}</td>
                                        <td class="py-3.5 px-4 font-bold text-slate-800">${{ hist.amount }}</td>
                                        <td class="py-3.5 px-4"><span class="px-2.5 py-1 bg-emerald-50 text-emerald-600 font-bold text-xs rounded-lg">Paid</span></td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

            </div>
        </main>

        <!-- MODAL FOR FOOD DETAILS / ADD / EDIT -->
        <div v-if="modal.show" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
            <div class="bg-white rounded-3xl shadow-2xl max-w-lg w-full overflow-hidden animate-in fade-in zoom-in duration-200">
                <div class="p-6 border-b border-slate-100 flex items-center justify-between">
                    <h3 class="font-bold text-slate-800 text-lg">{{ modal.mode === 'view' ? 'Food / Drink Details' : (modal.mode === 'add' ? 'Add New Menu Item' : 'Edit Menu Item') }}</h3>
                    <button @click="closeModal" class="p-2 text-slate-400 hover:text-slate-600 rounded-lg">
                        <i class="fa-solid fa-xmark text-lg"></i>
                    </button>
                </div>
                
                <div class="p-6 space-y-4">
                    <div v-if="modal.mode === 'view'">
                        <img :src="modal.data.image" class="w-full h-48 object-cover rounded-2xl mb-4 shadow" alt="Item">
                        <h4 class="font-extrabold text-2xl text-slate-800">{{ modal.data.name }}</h4>
                        <span class="inline-block mt-1 px-3 py-1 bg-emerald-100 text-emerald-700 text-xs font-bold rounded-lg">{{ modal.data.category }}</span>
                        <p class="text-sm text-slate-600 mt-4">{{ modal.data.description }}</p>
                        <div class="mt-6 flex items-center justify-between p-4 bg-slate-50 rounded-xl border border-slate-200">
                            <span class="text-sm font-bold text-slate-600">Price</span>
                            <span class="text-xl font-black text-emerald-600">${{ modal.data.price }}</span>
                        </div>
                    </div>

                    <div v-else class="space-y-4">
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Item Name</label>
                            <input v-model="modal.data.name" type="text" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Category</label>
                                <select v-model="modal.data.category" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                                    <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Price ($)</label>
                                <input v-model.number="modal.data.price" type="number" step="0.01" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Image URL</label>
                            <input v-model="modal.data.image" type="text" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Description</label>
                            <textarea v-model="modal.data.description" rows="3" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500"></textarea>
                        </div>
                    </div>
                </div>

                <div class="p-6 bg-slate-50 border-t border-slate-100 flex justify-end gap-3">
                    <button @click="closeModal" class="px-5 py-2.5 bg-slate-200 hover:bg-slate-300 text-slate-700 font-semibold text-sm rounded-xl transition-all">Cancel</button>
                    <button v-if="modal.mode !== 'view'" @click="saveFoodItem" class="px-5 py-2.5 bg-emerald-600 hover:bg-emerald-700 text-white font-semibold text-sm rounded-xl transition-all shadow-md">Save Item</button>
                </div>
            </div>
        </div>

    </div>
</div>
</template>

<script setup>
import { Chart } from 'chart.js/auto'

// Navigation state
const currentTab = ref('status')
const searchQuery = ref('')
const selectedCategoryFilter = ref('All')

// Toast state
const toast = ref({
  show: false,
  message: '',
  type: 'success'
})

const showToast = (msg, type = 'success') => {
  toast.value = {
    show: true,
    message: msg,
    type
  }

  setTimeout(() => {
    toast.value.show = false
  }, 3500)
}


// Restaurant Identity & Settings
const restaurant = ref({
  name: 'Kaffa Gourmet & Lounge',
  location: 'Bole Road, Addis Ababa',
  phone: '+251 911 223344',
  description:
    'Premium fine dining restaurant serving exquisite local and continental delicacies with state-of-the-art QR code ordering.',
  image:
    'https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?auto=format&fit=crop&q=80&w=400'
})

const settings = ref({
  orderingOption: true,
  roomIntegration: true,
  restaurantBranch: false
})


// Categories & Foods
const categories = ref([
  'Main Course',
  'Appetizers',
  'Beverages',
  'Desserts',
  'Cocktails'
])

const tablesCount = ref(24)
const roomsCount = ref(18)

const foods = ref([
  {
    id: 1,
    name: 'Truffle Wagyu Burger',
    category: 'Main Course',
    price: 24.99,
    description:
      'Juicy wagyu beef patty topped with black truffle mayo, aged cheddar, and crispy onion strings.',
    image:
      'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?auto=format&fit=crop&q=80&w=400',
    views: 4200,
    orders: 340,
    avgTime: '2m 15s'
  },
  {
    id: 2,
    name: 'Artisan Margherita Pizza',
    category: 'Main Course',
    price: 18.5,
    description:
      'Wood-fired sourdough pizza with San Marzano tomatoes, fresh buffalo mozzarella, and basil.',
    image:
      'https://images.unsplash.com/photo-1604382355076-af4b0eb60143?auto=format&fit=crop&q=80&w=400',
    views: 3900,
    orders: 290,
    avgTime: '1m 50s'
  },
  {
    id: 3,
    name: 'Crispy Calamari Rings',
    category: 'Appetizers',
    price: 14,
    description:
      'Tender squid rings lightly battered and fried, served with zesty garlic aioli and lemon wedges.',
    image:
      'https://images.unsplash.com/photo-1599487488170-d3c11c07a486?auto=format&fit=crop&q=80&w=400',
    views: 2800,
    orders: 210,
    avgTime: '1m 20s'
  },
  {
    id: 4,
    name: 'Signature Mojito',
    category: 'Beverages',
    price: 9.5,
    description:
      'Refreshing blend of white rum, fresh mint leaves, lime juice, cane sugar, and sparkling soda.',
    image:
      'https://images.unsplash.com/photo-1551024709-8f23befc6f87?auto=format&fit=crop&q=80&w=400',
    views: 3100,
    orders: 450,
    avgTime: '1m 10s'
  },
  {
    id: 5,
    name: 'Molten Lava Chocolate Cake',
    category: 'Desserts',
    price: 11,
    description:
      'Warm chocolate cake with a gooey molten center, served alongside creamy vanilla bean gelato.',
    image:
      'https://images.unsplash.com/photo-1606313564200-e75d5e30476c?auto=format&fit=crop&q=80&w=400',
    views: 2400,
    orders: 180,
    avgTime: '2m 40s'
  },
  {
    id: 6,
    name: 'Smoked Old Fashioned',
    category: 'Cocktails',
    price: 15,
    description:
      'Small batch bourbon stirred with bitters and sugar cube, smoked with aromatic oak woodchips.',
    image:
      'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?auto=format&fit=crop&q=80&w=400',
    views: 1900,
    orders: 130,
    avgTime: '1m 45s'
  }
])


// Notifications / Customer Comments
const notifications = ref([
  {
    id: 1,
    author: 'Alex Turner',
    foodName: 'Truffle Wagyu Burger',
    comment:
      'Absolute perfection! The truffle mayo takes it to a whole new level.',
    time: '10 mins ago',
    rating: 5,
    unread: true
  },
  {
    id: 2,
    author: 'Sarah Jenkins',
    foodName: 'Artisan Margherita Pizza',
    comment:
      'Crust was exceptionally crispy and authentic. Loved every bite.',
    time: '45 mins ago',
    rating: 4.8,
    unread: true
  },
  {
    id: 3,
    author: 'Michael Chang',
    foodName: 'Signature Mojito',
    comment:
      'Very refreshing on a warm afternoon! Great mint aroma.',
    time: '2 hours ago',
    rating: 5,
    unread: false
  },
  {
    id: 4,
    author: 'Elena Rostova',
    foodName: 'Molten Lava Chocolate Cake',
    comment:
      'Divine chocolate flavor, though it could use slightly more ice cream.',
    time: 'Yesterday',
    rating: 4.5,
    unread: false
  }
])

const unreadCount = computed(() =>
  notifications.value.filter(n => n.unread).length
)

const markAllNotificationsRead = () => {
  notifications.value.forEach(n => {
    n.unread = false
  })

  showToast('All notifications marked as read.')
}


// Analytics
const totalSales = ref(18450)
const totalOrders = ref(1420)

const avgFoodPrice = computed(() => {
  if (foods.value.length === 0) return 0

  const sum = foods.value.reduce((acc, f) => acc + f.price, 0)

  return (sum / foods.value.length).toFixed(2)
})

const topViewedFoods = computed(() => {
  return [...foods.value]
    .sort((a, b) => b.views - a.views)
    .slice(0, 10)
})


// Filtered foods
const filteredFoods = computed(() => {
  let result = foods.value

  if (selectedCategoryFilter.value !== 'All') {
    result = result.filter(
      f => f.category === selectedCategoryFilter.value
    )
  }

  if (searchQuery.value.trim() !== '') {
    const q = searchQuery.value.toLowerCase()

    result = result.filter(
      f =>
        f.name.toLowerCase().includes(q) ||
        f.description.toLowerCase().includes(q)
    )
  }

  return result
})


// Modal state
const modal = ref({
  show: false,
  mode: 'view',
  data: {}
})

const viewFoodDetails = food => {
  modal.value = {
    show: true,
    mode: 'view',
    data: { ...food }
  }
}

const openAddFoodModal = () => {
  modal.value = {
    show: true,
    mode: 'add',
    data: {
      name: '',
      category: 'Main Course',
      price: 10,
      description: '',
      image:
        'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&q=80&w=400'
    }
  }
}

const editFood = food => {
  modal.value = {
    show: true,
    mode: 'edit',
    data: { ...food }
  }
}

const saveFoodItem = () => {
  if (!modal.value.data.name || !modal.value.data.price) {
    showToast('Please fill in required fields.', 'error')
    return
  }

  if (modal.value.mode === 'add') {
    const newId =
      Math.max(...foods.value.map(f => f.id), 0) + 1

    foods.value.push({
      ...modal.value.data,
      id: newId,
      views: 100,
      orders: 12,
      avgTime: '1m 30s'
    })

    showToast('New food item added successfully!')
  }

  else if (modal.value.mode === 'edit') {
    const idx = foods.value.findIndex(
      f => f.id === modal.value.data.id
    )

    if (idx !== -1) {
      foods.value[idx] = {
        ...modal.value.data
      }

      showToast('Food item updated successfully!')
    }
  }

  modal.value.show = false
}

const deleteFood = id => {
  if (confirm('Are you sure you want to delete this item?')) {
    foods.value = foods.value.filter(f => f.id !== id)

    showToast('Food item deleted.')
  }
}

const saveSettings = () => {
  showToast('Restaurant identity & toggles saved successfully!')
}


// Subscription
const subscription = ref({
  currentPlan: 'Pro Restaurant',
  price: 79,
  status: 'Active (Auto-Renewing)',
  nextPayment: 'October 15, 2026',

  plans: [
    {
      name: 'Starter',
      price: 29,
      desc: 'Ideal for small cafes and single station dining.',
      features: [
        'Up to 30 Menu Items',
        'QR Table Ordering',
        'Standard Support'
      ]
    },
    {
      name: 'Pro Restaurant',
      price: 79,
      desc: 'Great for busy restaurants with room service & branches.',
      features: [
        'Unlimited Menu Items',
        'Table & Room QR Integration',
        'Multi-Branch Support',
        'Priority Analytics'
      ]
    },
    {
      name: 'Enterprise',
      price: 199,
      desc: 'For large hospitality chains and luxury hotels.',
      features: [
        'Custom POS Integration',
        'Dedicated Account Manager',
        'Advanced API Access',
        '24/7 VIP Support'
      ]
    }
  ],

  history: [
    {
      id: 'INV-2026-09',
      date: 'Sep 15, 2026',
      plan: 'Pro Restaurant',
      amount: 79
    },
    {
      id: 'INV-2026-08',
      date: 'Aug 15, 2026',
      plan: 'Pro Restaurant',
      amount: 79
    },
    {
      id: 'INV-2026-07',
      date: 'Jul 15, 2026',
      plan: 'Pro Restaurant',
      amount: 79
    }
  ]
})

const changePlan = planName => {
  const found = subscription.value.plans.find(
    p => p.name === planName
  )

  if (found) {
    subscription.value.currentPlan = found.name
    subscription.value.price = found.price

    showToast(
      `Successfully upgraded to ${planName} plan!`
    )
  }
}


// Logout
const logout = () => {
  showToast('Logged out successfully.')

  setTimeout(() => {
    window.location.reload()
  }, 1000)
}


// Modal
const closeModal = () => {
  modal.value.show = false
}


// Current tab title
const currentTabTitle = computed(() => {
  switch (currentTab.value) {
    case 'status':
      return 'Status Dashboard'

    case 'items':
      return 'Menu Items Management'

    case 'notification':
      return 'Activity & Notifications'

    case 'analytics':
      return 'Performance Analytics'

    case 'subscription':
      return 'Subscription & Billing'

    case 'settings':
      return 'Restaurant Settings'

    default:
      return 'Dashboard'
  }
})


// Chart.js
const initCharts = () => {
  nextTick(() => {

    // Status Chart
    if (currentTab.value === 'status') {
      const ctx1 = document.getElementById('statusChart')

      if (ctx1) {
        new Chart(ctx1, {
          type: 'line',

          data: {
            labels: [
              'Mon',
              'Tue',
              'Wed',
              'Thu',
              'Fri',
              'Sat',
              'Sun'
            ],

            datasets: [
              {
                label: 'This Month',
                data: [
                  1200,
                  1900,
                  1500,
                  2200,
                  2800,
                  3500,
                  4200
                ],
                borderColor: '#10b981',
                backgroundColor: 'rgba(16, 185, 129, 0.1)',
                fill: true,
                tension: 0.4
              },

              {
                label: 'Last Month',
                data: [
                  1000,
                  1400,
                  1300,
                  1800,
                  2400,
                  3100,
                  3800
                ],
                borderColor: '#64748b',
                borderDash: [5, 5],
                tension: 0.4
              }
            ]
          },

          options: {
            responsive: true,
            maintainAspectRatio: false,

            plugins: {
              legend: {
                position: 'bottom'
              }
            }
          }
        })
      }
    }


    // Analytics Charts
    else if (currentTab.value === 'analytics') {

      const ctx2 = document.getElementById('visitorChart')

      if (ctx2) {
        new Chart(ctx2, {
          type: 'bar',

          data: {
            labels: [
              'Mon',
              'Tue',
              'Wed',
              'Thu',
              'Fri',
              'Sat',
              'Sun'
            ],

            datasets: [
              {
                label: 'Daily Visitors',
                data: [
                  650,
                  720,
                  810,
                  950,
                  1200,
                  1650,
                  1400
                ],
                backgroundColor: '#10b981',
                borderRadius: 8
              }
            ]
          },

          options: {
            responsive: true,
            maintainAspectRatio: false
          }
        })
      }


      const ctx3 = document.getElementById('peakChart')

      if (ctx3) {
        new Chart(ctx3, {
          type: 'line',

          data: {
            labels: [
              '12 PM',
              '2 PM',
              '4 PM',
              '6 PM',
              '8 PM',
              '10 PM'
            ],

            datasets: [
              {
                label: 'Traffic Intensity',
                data: [
                  200,
                  450,
                  300,
                  850,
                  1400,
                  900
                ],
                borderColor: '#6366f1',
                backgroundColor: 'rgba(99, 102, 241, 0.1)',
                fill: true,
                tension: 0.4
              }
            ]
          },

          options: {
            responsive: true,
            maintainAspectRatio: false
          }
        })
      }
    }
  })
}

// Watch tab changes
watch(currentTab, () => {
  initCharts()
})


// Initialize when page mounts
onMounted(() => {
  initCharts()
})
</script>