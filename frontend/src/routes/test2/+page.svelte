<script>
  import { pie, arc } from 'd3';
  import { Button, Checkbox, Dropdown } from "flowbite-svelte";
  import model from '../../lib/model.json';
    
  const data = [
    { percent: 50 },
    { percent: 100-50 }
  ];

  const y = Object.keys(data[0])[0]; // given d in data, returns the (quantitative) y-value
  
  let yVals = data.map((el) => Number(el[y]));
  
  const total = yVals.reduce((a, b) => a + b, 0);
  yVals = yVals.map((el) => el / total);
  
  const iVals = data.map((el, i) => i);

  // colors can be adjusted manually by creating a color array which length matches length of data set.
  let colors = ['#C3B6F7', '#FBFAFF']

  const wedges = pie().
    padAngle(0).
    sort(null).
    value(i => yVals[i])(iVals);

  console.log(wedges);

  const arcPath = arc()
    .innerRadius(50)
    .outerRadius(80);
  
  let isDropdownOpen = true;

  function handleDropdownClick(DropdownOpen) {
    DropdownOpen = !DropdownOpen // togle state on click
  }

</script>


<div class="ml-16 mt-16" >
  <Button class="mt-5 cursor-pointer justify-start w-fit focus:outline-none focus:ring-transparent" on:click={handleDropdownClick(isDropdownOpen)}>
    <div class="drop-shadow-sm border-2 border-white py-1 px-4 flex w-fit rounded-full text-base text-violet-500 font-medium mt-0 bg-violet-300">
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
  </Button>
  <Dropdown class="-mt-4 ml-0 w-fit dropdown-content menu p-2 shadow bg-white rounded-lg">
    <li>
      <p class="ml-5 btn text-violet-800 font-semibold text-xl mt-2">Matched Genes & Percentage</p>
      <div class="flex">
        <div class="relative">
          <svg class="drop-shadow-md -m-10" width=280 height=280 viewBox="{-280 / 2} {-280 / 2} {280} {280}">
            {#each wedges as wedge, i}
              <path fill={colors[i]} d={arcPath(wedge)} />
            {/each}
          </svg>
          <div class="text-center absolute top-1/3 left-1/3 ml-1 mt-2">
            <p class="text-3xl font-semibold text-violet-800">
              50%
            </p>
            <p class="text-base font-semibold text-violet-800 -mt-2">
              matched
            </p>
          </div>
        </div>
        <div class="ml-3 bg-white w-44 overflow-y-auto py-1 h-48">
          {#each Object.keys(model["RPKM"]["ABL1"]) as gene, index}
            <div class="text-center mt-4 flex">
              <Checkbox
              id="{gene}_boxplot-check1"
              class="text-center cursor-pointer checked:bg-[#A68DF2] focus:ring-white"
              checked=true
              />
              <label class="cursor-pointer ml-5 text-neutral-400 text-sm font-medium" for="boxplot-check1">
                {Object.keys(model["RPKM"]["ABL1"])[index]}
              </label>
            </div>            
          {/each}
        </div>        
      </div>
    </li>
  </Dropdown>
</div>

<style>
  /* YourComponent에 대한 스타일 */
  @import '../../routes/scrollbar.css'; /* scrollbar.css 파일 임포트 */
</style>

