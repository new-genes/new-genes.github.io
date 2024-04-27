<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import { Button } from "flowbite-svelte";
  import { P, A } from "flowbite-svelte";
  import { Popover } from 'flowbite-svelte';
  import { Input } from 'flowbite-svelte';
  import model from '../../lib/model.json';

  let ABL1averageResultstr = [];
  let CRLF2averageResultstr = [];
  let ABL1_LikeaverageResultstr = [];
  let patientIDnumberstr = [];
  
  // 페이지당 결과 수 설정 
  let currentPage = 1; // 현재 페이지
  const pagesToShow = 5; // 페이지네이션에 표시할 페이지 수
  let totalPages = 0; // 총 페이지 수


  // 변수를 reactive하게 선언합니다.
  let ABL1averageResult = writable('');
  let CRLF2averageResult = writable('');
  let ABL1_LikeaverageResult = writable('');
  let ABL1selected = writable('');
  let CRLF2selected = writable('');
  let ABL1_LikeSelected = writable('');
  let selectedmethod = writable('');
  let patientIDnumber = writable('');

  let params;

  onMount(() => {
    params = new URLSearchParams(window.location.search);

    // 변수 값이 변경되면 store에 새로운 값을 설정합니다.
    ABL1averageResult.set(params.get('ABL1') || '');
    CRLF2averageResult.set(params.get('CRLF2') || '');
    ABL1_LikeaverageResult.set(params.get('ABL1_L') || '');
    ABL1selected.set(params.get('ABL1s') || '');
    CRLF2selected.set(params.get('CRLF2s') || '');
    ABL1_LikeSelected.set(params.get('ABL1_Ls') || '');
    selectedmethod.set(params.get('smthd') || '');
    patientIDnumber.set(params.get('PatID') || '');

  });

  // ABL1averageResult 값이 변경될 때마다 실행됩니다.
  ABL1averageResult.subscribe(value => {
    // value를 decode하고 배열에 추가하는 등의 작업을 수행합니다.
    let decodedArray = decodearray(value);
    console.log('ABL1 Average:', decodedArray);
    // 새로운 변수에 저장하려면 아래와 같이 할당합니다.
    ABL1averageResultstr = decodedArray;
    console.log('ABL1averageResultstr:', ABL1averageResultstr);
  });

  // CRLF2averageResult 값이 변경될 때마다 실행됩니다.
  CRLF2averageResult.subscribe(value => {
    let decodedArray = decodearray(value);
    console.log('CRLF2 Average:', decodedArray);
    CRLF2averageResultstr = decodedArray;
    console.log('CRLF2averageResultstr:', CRLF2averageResultstr);
  });

  // ABL1_LikeaverageResult 값이 변경될 때마다 실행됩니다.
  ABL1_LikeaverageResult.subscribe(value => {
    let decodedArray = decodearray(value);
    console.log('ABL1_Like Average:', decodedArray);
    ABL1_LikeaverageResultstr = decodedArray;
    console.log('ABL1_LikeaverageResultstr:', ABL1_LikeaverageResultstr);
  });

  // ABL1selected 값이 변경될 때마다 실행됩니다.
  ABL1selected.subscribe(value => {
    console.log('Selected ABL1:', value);
  });

  // CRLF2selected 값이 변경될 때마다 실행됩니다.
  CRLF2selected.subscribe(value => {
    console.log('Selected CRLF2:', value);
  });

  // ABL1_LikeSelected 값이 변경될 때마다 실행됩니다.
  ABL1_LikeSelected.subscribe(value => {
    console.log('Selected ABL1_Like:', value);
  });

  // selectedmethod 값이 변경될 때마다 실행됩니다.
  selectedmethod.subscribe(value => {
    console.log('Selected Method:', value);
  });

  // patientIDnumber 값이 변경될 때마다 실행됩니다.
  patientIDnumber.subscribe(value => {
    console.log('Patient ID:', value);
    // +를 기준으로 split하여 첫 번째 값을 제외하고, 마지막 값의 '\r'을 제거한 나머지 값을 patientIDnumberstr에 할당합니다.
    let splitValues = value.split('+').slice(1);
    patientIDnumberstr = splitValues.map(item => item.replace(/\r$/, ''));
    console.log('patientIDnumberstr:', patientIDnumberstr);
    let resultsPerPage = 1; // 한 페이지당 결과 수
    totalPages = Math.ceil(patientIDnumberstr.length / resultsPerPage); // 총 페이지 수
    console.log('Total Pages:', totalPages);
  });

  function decodearray(str) {
    let blob = atob(str);
    let ary_buf = new ArrayBuffer(blob.length);
    let dv = new DataView(ary_buf);
    for (let i = 0; i < blob.length; i++) dv.setUint8(i, blob.charCodeAt(i));

    // For WebGL Buffers, can skip Float32Array, just return ArrayBuffer is all thats needed.
    let f32_ary = new Float32Array(ary_buf);

    return f32_ary;
  }

  // 파일 선택 시 호출되는 함수
  function starlocation(number) {
    let result = parseInt((parseFloat(number) + 1) * 47.3 + 1.5);
    return result;
  }

  
  // 페이지 변경 함수
  function changePage(pageNumber) {
    if (pageNumber >= 1 && pageNumber <= totalPages) {
      currentPage = pageNumber;
      console.log('Current Page:', currentPage);
    }
  }

  // 페이지네이션에 표시될 페이지 번호 계산 함수
  function getPageNumbers() {
    const halfPagesToShow = Math.floor(pagesToShow / 2);
    let startPage = Math.max(currentPage - halfPagesToShow, 1);
    let endPage = Math.min(startPage + pagesToShow - 1, totalPages);

    // Adjust startPage and endPage when near the beginning or end of the page list
    if (totalPages - endPage < halfPagesToShow) {
      startPage = Math.max(endPage - pagesToShow + 1, 1);
    } else if (startPage <= halfPagesToShow) {
      endPage = Math.min(startPage + pagesToShow - 1, totalPages);
    }

    return Array.from({ length: endPage - startPage + 1 }, (_, i) => startPage + i);
  }

  // 이전 페이지 그룹으로 이동하는 함수
  function prevPageGroup() {
    if (currentPage > pagesToShow) {
      currentPage -= pagesToShow;
    } else {
      currentPage = 1;
    }
  }

  // 다음 페이지 그룹으로 이동하는 함수
  function nextPageGroup() {
    if (currentPage + pagesToShow <= totalPages) {
      currentPage += pagesToShow;
    } else {
      currentPage = totalPages;
    }
  }

  // 현재 페이지에서 표시할 결과 인덱스 계산
  function calculateIndex(currentPage) {
    return currentPage - 1;
  }

  function scrollToTop() {
    window.scrollTo({
      top: 0,
      behavior: 'smooth' // 부드러운 스크롤 적용
    });
  }

  let searchKeyword = '';

  function handleSearch() {
  // 검색어를 가져와서 searchKeyword 변수에 저장합니다.
  searchKeyword = document.getElementById('searchInput').value.trim().toLowerCase();
  // 페이지를 1페이지로 초기화합니다.
  changePage(1);
  }

  import { Dropdown, DropdownItem, Radio } from 'flowbite-svelte';
  let group2 = 1;

  import { Checkbox, Search } from 'flowbite-svelte';
  import { UserRemoveSolid } from 'flowbite-svelte-icons';

  let searchTerm = '';
  $: filteredItems = patientIDnumberstr.filter((item) => item.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1);
</script>

<style>
  /* YourComponent에 대한 스타일 */
  @import '../../routes/scrollbar.css'; /* scrollbar.css 파일 임포트 */
</style>

<div class="selection:bg-indigo-400 selection:text-white relative mt-12 rounded-lg border mx-5 px-8 pt-10 bg-white">
  <p class="ml-16 text-3xl text-violet-800 font-medium mt-8">Ph(+) B-ALL Probability Calculator</p>
  <div class="relative w-full px-10 pt-3">
    <p class="mt-8 ml-8 text-3xl text-violet-500 font-medium">Results</p>
    <p class="ml-8 text-violet-300 text-base font-normal mt-2">
      {$selectedmethod} Based Probability of Each Class
    </p>  
    {#if $patientIDnumber[calculateIndex(currentPage)]}
      <div class="drop-shadow-sm bg-violet-50 mx-10 rounded-2xl border px-5 pb-5 pt-3 mt-8 border-violet-50">
        <div class="ml-0 mr-5 justify-between flex rounded-3xl py-1 mt-0">
          <div class="rounded-2xl py-1 cursor-pointer -mt-1 w-52 flex text-left justify-start text-xl font-medium text-violet-300 h-full">
            <div class="rounded-2xl flex">
              <Button class="focus:ring-transparent text-violet-500 text-xl">Patient ID</Button>
              <img
              id="searchIcon"
              src="invertedtriangle2.svg"
              class="cursor-pointer w-4 h-4 mt-5 h-fit text-center"
              alt="Tutorial Logo"/> 
            </div>
          </div>
          <div class="text-left ml-5 mt-3 w-full">
            <Button class="drop-shadow-sm text-left -my-3 py-2 bg-[#FBFAFF] text-violet-400 focus:text-violet-500 text-base border-violet-200 focus:outline-none focus:border-2 focus:border-violet-100 focus:ring-1 focus:ring-violet-100 focus:bg-white rounded-full w-full">Click to find the PatientID!</Button>
            <Dropdown class="text-left overflow-y-auto px-3 pb-3 text-sm h-40 w-full ml-auto"> <!-- 드롭다운 요소에 ml-auto 클래스 추가 -->
              <div class="bg-white outline-none px-1" slot="header">
                <Search class="h-10 text-left bg-white text-neutral-500 focus:text-neutral-500 text-base border-white focus:outline-none focus:border-white focus:ring-1 focus:ring-white focus:bg-whiite" size="md"  bind:value={searchTerm} />
              </div>
              {#each filteredItems as patientid, num}
              <li class="text-left text-neutral-400 rounded rounded-full p-1 focus:ring-transparent hover:bg-violet-50 hover:text-violet-400 dark:hover:bg-gray-600">
                <Button class="focus:ring-transparent text-neutral-400 hover:text-violet-400 focus:text-violet-400 flex" 
                  on:click={() => {
                  changePage(patientIDnumberstr.indexOf(patientid)+1);
                  scrollToTop(); // 페이지 변경 시 맨 위로 스크롤
                }}>{patientid}
                </Button>
              </li>
              {/each}
              <a slot="footer" href="/" class="flex items-center p-3 -mb-1 text-sm font-medium text-red-600 bg-white hover:bg-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:text-red-500 hover:underline">
              </a>
            </Dropdown>
          </div>
          <div class="py-1 mt-1 flex items-center justify-end text-xl text-center font-medium mx-0 text-violet-400 h-full">
            <img
            id="searchicon2"
            src="searchicon3.svg"
            class="cursor-pointer w-5 h-5 my-1 ml-8 mr-0 mt-1 h-fit text-center"
            alt="Tutorial Logo"
            /> 
          </div>
        </div>
        <hr class="mb-5 mt-1 border-violet-100"/>
        <div class="mt-5 ml-3 mb-3 flex">
          <p class="ml-10 mt-16 justify-center text-2xl text-center font-semibold text-violet-800 font-medium mt-5">{patientIDnumberstr[calculateIndex(currentPage)]}'s Analysis Result</p>
        </div>
        <div class="cursor-pointer rounded-2xl justify-end text-lg mx-12 flex">
          <div class="drop-shadow-sm bg-white rounded-2xl px-3 py-1 mx-1 flex">
            <img
            id = "ABL1_star"
            src="Star_yellow3.svg"
            class="cursor-pointer w-4 h-4 mr-1 h-fit text-center"
            alt="Tutorial Logo2"
            />
            <p class="cursor-pointer text-xs font-semibold text-violet-300">ABL1 Class</p>
          </div>
          <div class="drop-shadow-sm bg-white ursor-pointer rounded-2xl px-3 py-1 mx-1 flex">
            <img
            id = "CRLF2_star"
            src="Star_red3.svg"
            class="cursor-pointer w-4 h-4 mr-1 h-fit text-center"
            alt="Tutorial Logo2"
            />
            <p class="cursor-pointer text-xs font-semibold text-violet-300">CRLF2 Class</p>
          </div>
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
        <div class="drop-shadow-sm bg-[#FBFAFF] mx-10 rounded-2xl px-12 py-5 mt-3">
          <div class="mx-5">
            <div class="drop-shadow-sm -mx-7 w-fit rounded-full p-2 mb-8 text-base text-violet-500 font-medium mt-0 bg-violet-100 border-2 border-white">
              <p class="mx-5 font-semibold">Total class</p>
            </div>
            <div class="relative mt-0 flex font-semibold">
              <p class="ml-4 text-xs text-indigo-500">BALLNOS</p>
              <p class="absolute right-2 text-xs text-pink-500">Other Classes</p>
            </div>
            <div class="drop-shadow-sm bg-white mt-1 ml-2 relative h-12 pt-2 flex rounded-full font-semibold text-sm text-white">
              <div class="text-center absolute left-[0%] -mt-2 h-12 w-1/2 bg-indigo-200 rounded-full border-2 border-white"><p class="text-indigo-500 mt-3 -ml-20">More Less Likely</p></div>
              <div class="text-center absolute left-[38.5%] -mt-2 h-12 w-[11%] bg-indigo-100 border-2 border-white"><p class="text-indigo-400 text-xs mt-2 ml-0">Less <br> Likely</p></div>
              <div class="text-center absolute right-[0%] -mt-2 h-12 w-1/2 bg-pink-200 rounded-full border-2 border-white"><p class="text-pink-500 mt-3 ml-16">High Confidence</p></div>
              <div class="text-center absolute right-[40%] -mt-2 h-12 w-[11%] bg-pink-100 border-2 border-white"><p class="text-pink-400 text-xs mt-2 ml-0">Low <br> Confidence</p></div>
            </div>  
            <div class="mb-5 relative mt-0 flex font-semibold text-xs">
              <p class="absolute left-1 text-left ml-3 text-indigo-400">-1</p>
              <p class="absolute ml-3 left-[37%] text-indigo-300">-0.2</p>
              <p class="absolute ml-3 left-[48%] text-neutral-300">0</p>
              <p class="absolute ml-3 left-[59%] text-pink-300">0.2</p>
              <p class="absolute right-5 text-right text-pink-400">1</p>
            </div>
          </div>
          <div class="-ml-3 bg-inherit w-full relative">
            {#if $ABL1selected == 'true'}
              <img
              id = "Total_ABL1"
              src="Star_yellow3.svg"
              class="drop-shadow-sm cursor-pointer absolute w-7 h-7 ml-3 -mt-[85px] h-fit text-center"
              style="left: {`${starlocation(ABL1averageResultstr[calculateIndex(currentPage)])}%`}"
              alt="Tutorial Logo"
              />
              <Popover triggeredBy="#Total_ABL1" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
                <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                  <img
                    id = "ABL1_star"
                    src="Star_yellow.svg"
                    class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                    alt="Tutorial Logo2"
                    />
                    <p class="text-sm text-violet-500 font-semibold">ABL1 Class</p>
                </div>
                <p class="px-2 text-xs text-violet-300">The probability of ABL1 class is <span class="ml-0 font-semibold text-violet-400 dark:text-white">{ABL1averageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
              </Popover>
            {/if}
            {#if $CRLF2selected == 'true'}
              <img
              id="Total_CRLF2"
              src="Star_red3.svg"
              class="drop-shadow-sm cursor-pointer absolute w-7 h-7 ml-3 -mt-[85px] h-fit text-center"
              style="left: {`${starlocation(CRLF2averageResultstr[calculateIndex(currentPage)])}%`}"
              alt="Tutorial Logo"
              />
              <Popover triggeredBy="#Total_CRLF2" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
                <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                  <img
                    id = "CRLF2_star"
                    src="Star_red.svg"
                    class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                    alt="Tutorial Logo2"
                    />
                    <p class="text-sm text-violet-500 font-semibold">CRLF2 Class</p>
                </div>
                <p class="px-2 text-xs text-violet-300">The probability of CRLF2 class is   <span class="ml-0 font-semibold text-violet-400 dark:text-white">{CRLF2averageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
              </Popover>
            {/if}
            {#if $ABL1_LikeSelected == 'true'}
            <img
            id="Total_ABL1_Like"
            src="Star_mint3.svg"
            class="drop-shadow-sm cursor-pointer absolute w-7 h-7 ml-3 -mt-[85px] h-fit text-center"
            style="left: {`${starlocation(ABL1_LikeaverageResultstr[calculateIndex(currentPage)])}%`}"
            alt="Tutorial Logo"
            />
            <Popover triggeredBy="#Total_ABL1_Like" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
              <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                <img
                  id = "ABL1_Like_star"
                  src="Star_mint.svg"
                  class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                  alt="Tutorial Logo2"
                  />
                  <p class="text-sm text-violet-500 font-semibold">ABL1 Like Class</p>
              </div>
              <p class="px-2 text-xs text-violet-300">The probability of ABL1 Like class is <span class="ml-0 font-semibold text-violet-400 dark:text-white">{ABL1_LikeaverageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
            </Popover>
            {/if}
          </div>
        </div>              
        <div class="drop-shadow-sm bg-[#FBFAFF] mx-10 rounded-2xl px-20 py-5 mt-8">
          {#if $ABL1selected == 'true'}
            <div class="my-3">
              <div>
                <div class="-mx-5 relative flex">
                  <div class="w-fit">
                    <div class="drop-shadow-sm border-2 border-white py-2 px-4 flex -mx-5 w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-100">
                      <p class="ml-3 text-base text-violet-500 font-semibold">ABL1 Class</p>
                      <p class="ml-1 mr-2 text-base text-violet-400 font-semibold">: {ABL1averageResultstr[calculateIndex(currentPage)].toFixed(4)}</p>
                    </div>
                  </div>
                  <div class="absolute right-0 mt-5 cursor-pointer justify-start w-fit">
                    <div class="drop-shadow-sm ml-10 border-2 border-white py-1 px-4 flex w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-300">
                      <img
                      id="Star_purple"
                      src="Star_violet.svg"
                      class="w-4 h-4 mr-2 text-center"
                      alt="Tutorial Logo"
                      />
                      <p class="text-white font-medium text-sm">5 out of 10 gene of the model matched</p>
                      <p class="ml-1 text-violet-500 text-sm font-semibold">(50%)</p>
                      <img
                        id = "ABL1_star"
                        src="under_arrow3.svg"
                        class="cursor-pointer mt-2 ml-2 w-3 h-3 mr-0 h-fit text-center"
                        alt="Tutorial Logo2"
                      />
                    </div>
                  </div>
                </div>
                <div class="relative font-semibold mt-12 flex">
                  <p class="ml-4 text-xs text-indigo-500">BALLNOS</p>
                  <p class="absolute right-2 text-xs text-pink-500">ABL1</p>
                </div>
                <div class="drop-shadow-sm bg-white mt-1 relative h-12 pt-2 flex rounded-full font-semibold text-sm text-white">
                  <div class="text-center absolute left-[0%] -mt-2 h-12 w-1/2 bg-indigo-200 rounded-full border-2 border-white"><p class="text-indigo-500 mt-3 -ml-20">More Less Likely</p></div>
                  <div class="text-center absolute left-[38.5%] -mt-2 h-12 w-[11%] bg-indigo-100 border-2 border-white"><p class="text-indigo-400 text-xs mt-2 ml-0">Less <br> Likely</p></div>
                  <div class="text-center absolute right-[0%] -mt-2 h-12 w-1/2 bg-pink-200 rounded-full border-2 border-white"><p class="text-pink-500 mt-3 ml-16">High Confidence</p></div>
                  <div class="text-center absolute right-[40%] -mt-2 h-12 w-[11%] bg-pink-100 border-2 border-white"><p class="text-pink-400 text-xs mt-2 ml-0">Low <br> Confidence</p></div>
                </div>   
                <div class="-ml-3 mb-7 relative mt-0 flex font-semibold text-xs">
                  <p class="absolute left-1 text-left ml-5 text-indigo-400">-1</p>
                  <p class="absolute ml-2 left-[37%] text-indigo-300">-0.2</p>
                  <p class="absolute ml-3 left-[48%] text-neutral-300">0</p>
                  <p class="absolute ml-2 left-[59%] text-pink-300">0.2</p>
                  <p class="absolute right-5 text-right text-pink-400">1</p>
                </div>
              </div>   
              <div class="my-2 -mt-0 mb-5 bg-inherit w-full relative">
                <img
                id="ABL1"
                src="Star_yellow3.svg"
                class="drop-shadow-sm cursor-pointer absolute w-7 h-7 -ml-1 -mt-[92px] h-fit text-center"
                style="left: {`${starlocation(ABL1averageResultstr[calculateIndex(currentPage)])}%`}"
                alt="Tutorial Logo"
                />
                <Popover triggeredBy="#ABL1" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
                  <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                    <img
                      id = "ABL1_star"
                      src="Star_yellow.svg"
                      class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                      alt="Tutorial Logo2"
                      />
                      <p class="text-sm text-violet-500 font-semibold">ABL1 Class</p>
                  </div>
                  <p class="px-2 text-xs text-violet-300">The probability of ABL1 class is <span class="ml-0 font-semibold text-violet-400 dark:text-white">{ABL1averageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
                </Popover>
              </div>
              <hr class="-mx-10 mt-16 mb-12 border-violet-100"/>
            </div>              
          {/if}
          {#if $CRLF2selected == 'true'}
            <div class="mb-10">
              <div>
                <div class="-mx-5 relative flex">
                  <div class="w-fit">
                    <div class="drop-shadow-sm border-2 border-white py-2 px-4 flex -mx-5 w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-100">
                      <p class="ml-3 text-base text-violet-500 font-semibold">CRLF2 Class</p>
                      <p class="ml-1 mr-2 text-base text-violet-400 font-semibold">: {CRLF2averageResultstr[calculateIndex(currentPage)].toFixed(4)}</p>
                    </div>
                  </div>
                  <div class="-mr-5 absolute right-0 mt-5 cursor-pointer justify-start w-fit">
                    <div class="drop-shadow-sm ml-10 border-2 border-white py-1 px-4 flex w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-300">
                      <img
                      id="Star_purple"
                      src="Star_violet.svg"
                      class="w-4 h-4 mr-2 text-center"
                      alt="Tutorial Logo"
                      />
                      <p class="text-white font-medium text-sm">5 out of 10 gene of the model matched</p>
                      <p class="ml-1 text-violet-500 text-sm font-semibold">(50%)</p>
                      <img
                        id = "ABL1_star"
                        src="under_arrow3.svg"
                        class="cursor-pointer mt-2 ml-2 w-3 h-3 mr-0 h-fit text-center"
                        alt="Tutorial Logo2"
                      />
                    </div>
                  </div>
                </div>      
                <div class="relative font-semibold mt-10 flex">
                  <p class="ml-4 text-xs text-indigo-500">BALLNOS</p>
                  <p class="absolute right-2 text-xs text-pink-500">CRLF2</p>
                </div>
                <div class="drop-shadow-sm bg-white mt-1 relative h-12 pt-2 flex rounded-full font-semibold text-sm text-white">
                  <div class="text-center absolute left-[0%] -mt-2 h-12 w-1/2 bg-indigo-200 rounded-full border-2 border-white"><p class="text-indigo-500 mt-3 -ml-20">More Less Likely</p></div>
                  <div class="text-center absolute left-[38.5%] -mt-2 h-12 w-[11%] bg-indigo-100 border-2 border-white"><p class="text-indigo-400 text-xs mt-2 ml-0">Less <br> Likely</p></div>
                  <div class="text-center absolute right-[0%] -mt-2 h-12 w-1/2 bg-pink-200 rounded-full border-2 border-white"><p class="text-pink-500 mt-3 ml-16">High Confidence</p></div>
                  <div class="text-center absolute right-[40%] -mt-2 h-12 w-[11%] bg-pink-100 border-2 border-white"><p class="text-pink-400 text-xs mt-2 ml-0">Low <br> Confidence</p></div>
                </div>  
                <div class="-ml-3 mb-7 relative mt-0 flex font-semibold text-xs">
                  <p class="absolute left-1 text-left ml-5 text-indigo-400">-1</p>
                  <p class="absolute ml-2 left-[37%] text-indigo-300">-0.2</p>
                  <p class="absolute ml-3 left-[48%] text-neutral-300">0</p>
                  <p class="absolute ml-2 left-[59%] text-pink-300">0.2</p>
                  <p class="absolute right-5 text-right text-pink-400">1</p>
                </div>
              </div>   
              <div class="my-2 -mt-0 mb-5 bg-inherit w-full relative">
                <img
                id="CRLF2"
                src="Star_red3.svg"
                class="drop-shadow-sm cursor-pointer absolute w-7 h-7 -ml-1 -mt-[92px] h-fit text-center"
                style="left: {`${starlocation(CRLF2averageResultstr[calculateIndex(currentPage)])}%`}"
                alt="Tutorial Logo"
                />
                <Popover triggeredBy="#CRLF2" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
                  <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                    <img
                      id = "CRLF2_star"
                      src="Star_red.svg"
                      class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                      alt="Tutorial Logo2"
                      />
                      <p class="text-sm text-violet-500 font-semibold">CRLF2 Class</p>
                  </div>
                  <p class="px-2 text-xs text-violet-300">The probability of CRLF2 class is   <span class="ml-0 font-semibold text-violet-400 dark:text-white">{CRLF2averageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
                </Popover>
              </div>
              <hr class="-mx-10 mt-16 mb-12 border-violet-100"/>
            </div>            
          {/if}  
          {#if $ABL1_LikeSelected == 'true'}
            <div class="my-12">
              <div class="-mx-5 relative flex">
                <div class="w-fit">
                  <div class="drop-shadow-sm border-2 border-white py-2 px-4 flex -mx-5 w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-100">
                    <p class="ml-3 text-base text-violet-500 font-semibold">ABL1-Like Class</p>
                    <p class="ml-1 mr-2 text-base text-violet-400 font-semibold">: {ABL1_LikeaverageResultstr[calculateIndex(currentPage)].toFixed(4)}</p>
                  </div>
                </div>
                <div class="-mr-5 absolute right-0 mt-5 cursor-pointer justify-start w-fit">
                  <div class="drop-shadow-sm ml-10 border-2 border-white py-1 px-4 flex w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-300">
                    <img
                    id="Star_purple"
                    src="Star_violet.svg"
                    class="w-4 h-4 mr-2 text-center"
                    alt="Tutorial Logo"
                    />
                    <p class="text-white font-medium text-sm">5 out of 10 gene of the model matched</p>
                    <p class="ml-1 text-violet-500 text-sm font-semibold">(50%)</p>
                    <img
                        id = "ABL1_star"
                        src="under_arrow3.svg"
                        class="cursor-pointer mt-2 ml-2 w-3 h-3 mr-0 h-fit text-center"
                        alt="Tutorial Logo2"
                      />
                  </div>
                </div>
              </div>      
              <div class="relative font-semibold mt-12 flex">
                <p class="ml-4 text-xs text-indigo-500">BALLNOS</p>
                <p class="absolute right-2 text-xs text-pink-500">ABL1-Like</p>
              </div>
              <div class="drop-shadow-sm bg-white mt-1 relative h-12 pt-2 flex rounded-full font-semibold text-sm text-white">
                <div class="text-center absolute left-[0%] -mt-2 h-12 w-1/2 bg-indigo-200 rounded-full border-2 border-white"><p class="text-indigo-500 mt-3 -ml-20">More Less Likely</p></div>
                <div class="text-center absolute left-[38.5%] -mt-2 h-12 w-[11%] bg-indigo-100 border-2 border-white"><p class="text-indigo-400 text-xs mt-2 ml-0">Less <br> Likely</p></div>
                <div class="text-center absolute right-[0%] -mt-2 h-12 w-1/2 bg-pink-200 rounded-full border-2 border-white"><p class="text-pink-500 mt-3 ml-16">High Confidence</p></div>
                <div class="text-center absolute right-[40%] -mt-2 h-12 w-[11%] bg-pink-100 border-2 border-white"><p class="text-pink-400 text-xs mt-2 ml-0">Low <br> Confidence</p></div>
              </div>  
              <div class="-ml-3 mb-7 relative mt-0 flex font-semibold text-xs">
                <p class="absolute left-1 text-left ml-5 text-indigo-400">-1</p>
                <p class="absolute ml-2 left-[37%] text-indigo-300">-0.2</p>
                <p class="absolute ml-3 left-[48%] text-neutral-300">0</p>
                <p class="absolute ml-2 left-[59%] text-pink-300">0.2</p>
                <p class="absolute right-5 text-right text-pink-400">1</p>
              </div>
              
              <div class="mt-2 bg-inherit w-full relative">
                <img
                id="ABL1_Like"
                src="Star_mint3.svg"
                class="drop-shadow-sm cursor-pointer absolute w-7 h-7 -ml-1 -mt-[92px] h-fit text-center"
                style="left: {`${starlocation(ABL1_LikeaverageResultstr[calculateIndex(currentPage)])}%`}"
                alt="Tutorial Logo"
                />
                <Popover triggeredBy="#ABL1_Like" class="rounded rounded-2xl bg-white z-40 border border-white p-1 text-sm w-68 font-light">
                  <div class="justify-start rounded rounded-full bg-violet-50 mb-2 py-2 pl-5 pr-10 flex -mx-2 -mt-1">
                    <img
                      id = "ABL1_Like_star"
                      src="Star_mint.svg"
                      class="cursor-pointer w-4 h-4 mr-2 h-fit text-center"
                      alt="Tutorial Logo2"
                      />
                      <p class="text-sm text-violet-500 font-semibold">ABL1 Like Class</p>
                  </div>
                  <p class="px-2 text-xs text-violet-300">The probability of ABL1 Like class is <span class="ml-0 font-semibold text-violet-400 dark:text-white">{ABL1_LikeaverageResultstr[calculateIndex(currentPage)].toFixed(4)}</span>.</p>
                </Popover>
              </div>
            </div>              
          {/if}
        </div>
        
        <!-- 페이지네이션 UI -->
        <div class="flex justify-center items-center mt-5 mb-0 h-12">
          <!-- 이전 페이지 그룹 버튼 -->
          <button
            class="cursor-pointer text-violet-800 mx-1 px-3 py-1 focus:outline-none focus:border-violet-500"
            on:click={() => prevPageGroup()}
            disabled={currentPage === 1}>
            <img
            src="left2.svg"
            class="mx-5 h-7"
            alt="SPADOMA Logo"/>
          </button>
          <!-- 페이지 버튼 -->
          {#each getPageNumbers() as pageNumber}
            {#if currentPage === pageNumber}
              <button
                class="font-semibold text-violet-300 rounded-full text-lg mx-2 px-4 py-2 focus:outline-none bg-violet-300 text-violet-700"
                on:click={() => {
                  changePage(pageNumber);
                  scrollToTop(); // 페이지 변경 시 맨 위로 스크롤
                }}
              >
                {pageNumber}
              </button>
            {:else}
              <button
                class="font-semibold text-violet-300 rounded-full text-lg mx-2 px-4 py-2 focus:outline-none hover:text-violet-700 hover:bg-violet-300"
                on:click={() => {
                  changePage(pageNumber);
                  scrollToTop(); // 페이지 변경 시 맨 위로 스크롤
                }}
              >
                {pageNumber}
              </button>
            {/if}
          {/each}
          <!-- 다음 페이지 그룹 버튼 -->
          <button
            class="cursor-pointer mx-1 px-3 py-1 focus:outline-none focus:border-violet-500"
            on:click={() => nextPageGroup()}
            disabled={currentPage === totalPages}>
            <img
            src="right2.svg"
            class="mx-5 h-7"
            alt="SPADOMA Logo"/>
          </button>
        </div>
      </div>
      
    {:else}
      <p class="mx-10 mt-8 text-center text-gray-500">No results found for this page.</p>
    {/if}

    
    <div class="mt-12 mb-8 text-center">
      <Button
      href="/analysis"
      class="text-violet-100 mb-5 px-7 py-4 text-xl font-semibold bg-violet-800 ring ring-violet-300 hover:bg-violet-700 focus:ring-white"
      >Return</Button>
    </div>
    <footer>
      <div class="mt-16 mb-2 px-2 sm:px-4">
          <div class="mx-auto flex flex-col container">
              <P class="text-violet-400 text-center">This website is maintained by <A class="text-violet-600 underline" href="https://pnucolab.com/" target="_blank">Computational Omics Lab</A>, Pusan National University College of Biomedical Convergence Engineering, South Korea. </P>
          </div>
      </div>
    </footer>
  </div>
</div>


