<script>
  import { onMount } from 'svelte';

  let csvData = '';

  // CSV 파일을 가져오는 함수
  async function fetchCSV() {
    try {
      const response = await fetch('/total_fpkm_uq.csv'); // CSV 파일의 경로
      if (!response.ok) {
        throw new Error('파일을 불러오는 데 실패했습니다.');
      }
      csvData = await response.text(); // CSV 데이터를 텍스트로 변환하여 저장
      console.log(csvData);
    } catch (error) {
      console.error(error.message);
    }
  }

  onMount(fetchCSV); 
</script>



