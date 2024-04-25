<script>
  import { onMount } from 'svelte';

  let csvData = ''; // CSV 데이터를 저장할 변수
  let parsedData = []; // 파싱된 데이터를 저장할 2차원 배열

  async function fetchCSV() {
    try {
      const response = await fetch('/total_fpkm_uq.csv'); // CSV 파일의 경로
      if (!response.ok) {
        throw new Error('파일을 불러오는 데 실패했습니다.');
      }
      csvData = await response.text(); // CSV 데이터를 텍스트로 변환하여 저장
      parseCSV(); // CSV 데이터 파싱
      console.log(parsedData)
    } catch (error) {
      console.error(error.message);
    }
  }

  // CSV 데이터를 행(row)과 열(column)로 파싱하는 함수
function parseCSV() {
  parsedData = csvData.split('\r\n') // 각 줄을 배열로 분할
                     .map(row => row.split(',')) // 쉼표로 구분된 값들을 추출하여 이차원 배열로 만듦
                     .map(row => row.map(val => {
                       // 숫자로 변환 가능한 경우에만 실수값으로 변환하여 저장
                       const parsedVal = parseFloat(val);
                       return isNaN(parsedVal) ? val : parsedVal;
                     }));
  // 맨 마지막 줄이 빈 문자열인 경우, 이를 제거합니다.
  if (parsedData[parsedData.length - 1].length === 1 && parsedData[parsedData.length - 1][0] === '') {
    parsedData.pop();
  }
}

onMount(fetchCSV); // 페이지가 로드될 때 CSV 파일을 가져옴
</script>


{#each parsedData.slice(0, 10) as row, rowIndex}
  <tr>  
    {#each row.slice(0, 10) as cell, cellIndex}
      <th class="text-center text-sm bg-transparent py-2 px-5 mr-5">
        {cell}
      </th>
    {/each}
  </tr>
{/each}
