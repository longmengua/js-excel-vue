<template>
  <div>
    <input type="file" id="fileInput" @change="handleFileChange" />
    <table style="font-size: 10px; text-align: left;">
      <thead>
        <tr>
          <th>Identity</th>
          <th>Name</th>
          <th>School</th>
          <th>Class</th>
          <th>Cell Phone</th>
          <th>Home Phone Number</th>
          <th>Source Branch Campus</th>
          <th>Responsible Branch Campus</th>
          <th>Salesperson</th>
          <th>Application Time</th>
          <th>Labels</th>
          <th>Courses</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td v-for="(data, index) in presaleData" :key="index">{{ data }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import * as XLSX from 'xlsx';

const presaleData = ref([]);

const handleFileChange = (event) => {
  const file = event.target.files?.[0];

  if (file) {
    const reader = new FileReader();

    reader.onload = (e) => {
      const data = e.target?.result;
      if (typeof data === 'string') {
        parseExcelData(data);
      }
    };

    reader.readAsBinaryString(file);
  }
};

const parseExcelData = (binaryData) => {
  const workbook = XLSX.read(binaryData, { type: 'binary' });
  const sheetName = workbook.SheetNames[0];
  const sheet = workbook.Sheets[sheetName];

  const parsedData = XLSX.utils.sheet_to_json(sheet, {
    header: 1,
    range: 1, // Start parsing from the second row (index 1)
  });

  console.log("=====excel data====", parsedData[0])

  presaleData.value = parsedData[0];
};
</script>

<style scoped>
/* Add any component-specific styles here */
</style>
