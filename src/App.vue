<template>
  <div id="app">
    <div class="h-16 bg-gray-900 flex items-center">
      <div class="text-white">
        Logo
      </div>
      <div class="ml-20 font-semibold text-lg text-white">
        <a href="#" class="hover:underline hover:text-gray-500">Home</a>
      </div>
    </div>
    <router-view class="container" />
  </div>
</template>
