<script>
  function encode2DArray(array2D) {
    // 2차원 배열을 1차원 Uint8Array로 변환
    const flattenedArray = array2D.flat();
    const uint = new Uint8Array(flattenedArray.length * 4);
    flattenedArray.forEach((value, index) => uint.set(new Uint8Array(new Float32Array([value]).buffer), index * 4));

    // Uint8Array를 Base64 문자열로 인코딩
    const str = btoa(String.fromCharCode.apply(null, uint));
    return str;
}

  // 예제 2차원 배열
  const array2D = [
      [1, 2, 3],
      [4, 5, 6],
      [7, 8, 9]
  ];

  // 2차원 배열 인코딩
  const encodedString = encode2DArray(array2D);
  console.log("Encoded:", encodedString);

</script>