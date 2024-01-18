<template>
  <footer>
    <div class="flex flex-col items-center bg-black justify-between text-center overflow-hidden">
      <div class="a-section-marquee-box">
        <h2 class="marquee-text w-full text-9xl text-white font-bold py-4 my-2">
          ONLY HOT<span class="text-black">s</span>
        </h2>
        <h2 class="marquee-text w-full text-9xl text-white font-bold py-4 my-2">
          ONLY HOT <span class="text-black">s</span>
        </h2>
        <h2 class="marquee-text w-full text-9xl text-white font-bold py-4 my-2">
          ONLY HOT <span class="text-black">s</span>
        </h2>
      </div>
      <span class="text-white text-sm"
        >© 2024 Sneakshop. All rights reserved by
        <a class="link__foter hover:underline" href="https://www.danakun.com/" target="_blank"
          >Dana Kun</a
        ></span
      >
      <p class="text-stone-500 text-sm pb-4">
        All trademarks and registered trademarks are the property of their respective owners.
      </p>
    </div>
  </footer>
</template>

<style scoped>
/* footer {
  width: 100%;
  height: 300px;
} */

.link__foter {
  text-underline-offset: 2px;
}

.a-section-marquee-box {
  height: 140px;
  display: flex;
  align-items: center;
  overflow: hidden;
  border: 1px solid #fff; /* same as bg */
  margin: 20px 0;
}
.a-section-marquee-box h2 {
  white-space: nowrap;
  text-transform: uppercase;
  font-size: 7em;
  font-weight: 100;
  flex-shrink: 0;
  padding: 0;
  width: max-content;
  display: flex;
  align-items: center;
  transform: translateX(0);
  animation: a-text-scroll 30s linear infinite;
}
@keyframes a-text-scroll {
  0% {
    transform: translate3d(-100%, 0, 0);
  }
  100% {
    transform: translate3d(0%, 0, 0);
  }
}
</style>
