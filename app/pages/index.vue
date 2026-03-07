<template>
  <div class="relative min-h-screen bg-[#0a0a0a] overflow-hidden">
    
    <div class="absolute inset-0 bg-grid"></div>

    <div class="absolute inset-0 bg-gradient-to-b from-transparent via-[#0a0a0a]/50 to-[#0a0a0a]"></div>

    <div class="relative z-10 flex flex-col items-center justify-center min-h-screen">
      <h1 class="text-white text-5xl font-bold tracking-wider">
        PORTFOLIO
      </h1>
      <p class="text-gray-400 mt-4">Tunggu desain dan animasinya...</p>
    </div>
    
  </div>
</template>

<style scoped>
.bg-grid {
  background-size: 70px 70px; 
  
  background-image: 
    linear-gradient(to right, rgba(255, 255, 255, 0.05) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  
  background-position: top center;
}
</style>