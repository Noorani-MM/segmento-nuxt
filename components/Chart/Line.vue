<template>
  <VueApexCharts :options="options" :series="series"></VueApexCharts>
</template>

<script setup>
import VueApexCharts from "vue3-apexcharts";

const porps = defineProps({
  series: {
    required: true,
  },
});

const options = {
  chart: {
    type: "line",
    zoom: {
      enabled: false,
    },
    toolbar: {
      show: false,
    },
  },

  dataLabels: {
    enabled: false,
  },
  stroke: {
    curve: "smooth",
    width: 1, // اضافه کردن عرض لاین‌ها به مقدار دلخواه (اینجا مثلاً 2)
  },
  markers: {
    size: 2,
    colors: ["#063468"], // تغییر رنگ مارکر‌ها به مشکی
    strokeColors: ["transparent"],
  },
  xaxis: {
    axisBorder: {
      show: true,
    },
    axisTicks: {
      show: true,
    },
    labels: {
      show: true,
    },
  },
  yaxis: {
    axisBorder: {
      show: true,
    },
    axisTicks: {
      show: true,
    },
    labels: {
      show: true,
    },
  },
  grid: {
    show: true,
    xaxis: {
        lines: {
            show: true
        }
    },   
    yaxis: {
        lines: {
            show: false
        }
    }, 
  },
  dataLabels: {
    enabled: false,
  },
};

</script>