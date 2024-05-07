<script>
let selectedColor = "#000000";

function handleColorChange(event) {
  selectedColor = event.target.value;
}

import { Label } from "flowbite-svelte";


let ABL1selectedColor = "#FFD32A";

function ABL1handleColorChange(event) {
  selectedColor = event.target.value;
}


let CRLF2selectedColor = "#000000";

function CRLF2handleColorChange(event) {
  selectedColor = event.target.value;
}


let ABL1_LikeselectedColor = "#000000";

function ABL1_LikehandleColorChange(event) {
  selectedColor = event.target.value;
}

</script>

<div class="mt-20">
  <input id="color_select" class="hidden" type="color" style={{display:"none"}} bind:value={selectedColor} on:change={handleColorChange}>
  <Label for="color_select">
    <svg width="100" height="100">
      <!-- 예시로 원을 그렸어요. -->
      <circle cx="50" cy="50" r="40" stroke="white" stroke-width="3" fill={selectedColor} />
    </svg>
  </Label>
</div>
<div class="cursor-pointer rounded-2xl justify-end text-lg mx-12 flex">
  <input id="ABL1color_select" class="hidden" type="color" style={{display:"none"}} bind:value={ABL1selectedColor} on:change={ABL1handleColorChange}>
  <Label for="ABL1color_select"> 
    <div class="drop-shadow-sm bg-white rounded-2xl px-3 py-1 mx-1 flex"> 
      <svg class="-mt-0" width="16" height="16" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
        <mask id="path-1-inside-1_634_1177" fill="white">
          <path d="M20 0L26.7984 12.2728L40 15.2786L31 25.8695L32.3607 40L20 34.2728L7.63932 40L9 25.8695L0 15.2786L13.2016 12.2728L20 0Z"/>
        </mask>
          <path d="M20 0L26.7984 12.2728L40 15.2786L31 25.8695L32.3607 40L20 34.2728L7.63932 40L9 25.8695L0 15.2786L13.2016 12.2728L20 0Z" fill="{ABL1selectedColor}"/>
          <path d="M20 0L21.7495 -0.969126L20 -4.12743L18.2505 -0.969126L20 0ZM26.7984 12.2728L25.0489 13.2419L25.4823 14.0243L26.3544 14.2228L26.7984 12.2728ZM40 15.2786L41.524 16.5737L43.6596 14.0607L40.444 13.3285L40 15.2786ZM31 25.8695L29.476 24.5744L28.9281 25.2191L29.0092 26.0612L31 25.8695ZM32.3607 40L31.5199 41.8147L34.6859 43.2816L34.3515 39.8083L32.3607 40ZM20 34.2728L20.8408 32.4581L20 32.0685L19.1592 32.4581L20 34.2728ZM7.63932 40L5.64853 39.8083L5.31407 43.2816L8.48014 41.8147L7.63932 40ZM9 25.8695L10.9908 26.0612L11.0719 25.2191L10.524 24.5744L9 25.8695ZM0 15.2786L-0.444017 13.3285L-3.65959 14.0607L-1.52404 16.5737L0 15.2786ZM13.2016 12.2728L13.6456 14.2228L14.5177 14.0243L14.9511 13.2419L13.2016 12.2728ZM18.2505 0.969126L25.0489 13.2419L28.5479 11.3036L21.7495 -0.969126L18.2505 0.969126ZM26.3544 14.2228L39.556 17.2287L40.444 13.3285L27.2424 10.3227L26.3544 14.2228ZM38.476 13.9835L29.476 24.5744L32.524 27.1646L41.524 16.5737L38.476 13.9835ZM29.0092 26.0612L30.3699 40.1917L34.3515 39.8083L32.9908 25.6778L29.0092 26.0612ZM33.2015 38.1853L20.8408 32.4581L19.1592 36.0874L31.5199 41.8147L33.2015 38.1853ZM19.1592 32.4581L6.7985 38.1853L8.48014 41.8147L20.8408 36.0874L19.1592 32.4581ZM9.63011 40.1917L10.9908 26.0612L7.00921 25.6778L5.64853 39.8083L9.63011 40.1917ZM10.524 24.5744L1.52404 13.9835L-1.52404 16.5737L7.47596 27.1646L10.524 24.5744ZM0.444017 17.2287L13.6456 14.2228L12.7576 10.3227L-0.444017 13.3285L0.444017 17.2287ZM14.9511 13.2419L21.7495 0.969126L18.2505 -0.969126L11.4521 11.3036L14.9511 13.2419Z" fill="white" mask="url(#path-1-inside-1_634_1177)"/>
      </svg>
      <p class="ml-1 cursor-pointer text-xs font-semibold text-violet-300">ABL1 Class</p>
    </div>
  </Label>
  <Label>
    <div class="drop-shadow-sm bg-white ursor-pointer rounded-2xl px-3 py-1 mx-1 flex">
      <img
      id = "CRLF2_star"
      src="Star_red3.svg"
      class="cursor-pointer w-4 h-4 mr-1 h-fit text-center"
      alt="Tutorial Logo2"
      />
      <p class="cursor-pointer text-xs font-semibold text-violet-300">CRLF2 Class</p>
    </div>  
  </Label> 
  <div class="drop-shadow-sm bg-white cursor-pointer rounded-2xl px-3 py-1 mx-1 flex">
    <img
    id = "ABL1_Like_star"
    src="Star_mint3.svg"
    class="cursor-pointer w-4 h-4 mr-1 h-fit text-center"
    alt="Tutorial Logo2"
    />
    <p class="cursor-pointer text-xs font-semibold text-violet-300">ABL1-Like Class</p>
  </div>
</div>

