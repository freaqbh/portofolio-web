<template>
  <div class="relative min-h-screen bg-gray-100 flex p-4 overflow-hidden">
    <button 
      @click="toggleSidebar" 
      class="relative flex-none z-40 text-black hover:text-gray-600 transition-colors duration-250 focus:outline-none"
    >
      <ChevronRight :size="32" :stroke-width="1.75"/>
    </button>

    <Transition 
      @enter="enterAnimatioin" 
      @leave="leaveAnimation"
      :css="false"
    >
      <aside 
        v-show="isSidebarOpen"
        class="fixed top-0 left-0 w-64 h-full bg-white shadow-xl p-6 z-50"
      >
        <h2 class="text-xl font-bold mt-12 mb-4">Navigasi Utama</h2>
        <nav class="space-y-2">
          <a href="#" class="block text-gray-600 hover:text-blue-600 py-1">Beranda</a>
          <a href="#" class="block text-gray-600 hover:text-blue-600 py-1">Profil</a>
          <a href="#" class="block text-gray-600 hover:text-blue-600 py-1">Pengaturan</a>
        </nav>
        <button 
          @click="toggleSidebar" 
          class="absolute top-4 right-4 text-black hover:text-gray-600 transition-colors duration-250 focus:outline-none"
        >
          <ChevronLeft :size="32" :stroke-width="1.75" />
        </button>
      </aside>
    </Transition>

    <main class="flex-1 p-6 lg:p-12 transition-all duration-300">
      <header class="mb-10 mt-12 lg:mt-0">
        <h1 class="text-3xl font-extrabold text-gray-900">Koleksi Proyek</h1>
        <p class="text-gray-500 mt-2">Daftar pekerjaan dan eksperimen terbaru.</p>
      </header>

      <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6">
        
        <article class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100 hover:shadow-md transition-shadow">
          <div class="w-full h-40 bg-gray-200 rounded-xl mb-4"></div> <h3 class="text-xl font-bold text-gray-800 mb-2">Aplikasi Akademik</h3>
          <p class="text-gray-600 text-sm mb-6 line-clamp-2">
            Sistem otomatisasi untuk mengelola dokumen dan persetujuan secara digital.
          </p>
          
          <button class="group inline-flex items-center gap-2 p-2 -ml-2 rounded-lg hover:bg-blue-50 transition-colors text-blue-600 font-medium">
            Lihat Detail
            <ArrowRight class="w-4 h-4 transition-transform duration-300 group-hover:translate-x-1" />
          </button>
        </article>

        </div>
    </main>


  </div>
</template>

<script setup>
import { ref } from 'vue';
import { 
    ChevronRight,
    ChevronLeft,
    ArrowRight
} from 'lucide-vue-next';

// State untuk melacak status sidebar
const isSidebarOpen = ref(false);

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

const getAnimate = async () => {
    const { animate } = await import ('animejs');
    return animate;
};

const enterAnimatioin = async (el, done) => {
    const animate = await getAnimate();
    animate(el, {
        translateX: ['-100%', '0%'],
        ease: 'outElastic(1, .8)',
        duration: 800,
        onComplete: done
    });
};

const leaveAnimation = async (el, done) => {
    const animate = await getAnimate();
    animate(el, {
        translateX: ['0%', '-100%'],
        ease: 'inExpo',
        duration: 300,
        onComplete: done
    });
};

</script>
