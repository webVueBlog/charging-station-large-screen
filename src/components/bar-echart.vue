<template>
  <div ref="divRef" :style="{ width: width, height: height }"></div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import useEchart from "@/hooks/useEchart";
let props = defineProps({
  width: {
    type: String,
    default: "100%",
  },
  height: {
    type: String,
    default: "100%",
  },
  echartDatas: {
    type: Array,
    default: function () {
      return [];
    },
  },
});

let divRef = ref(null);
let hyEchart = null;

watch(
  () => props.echartDatas,
  (newV, oldV) => {
    setupEchart(newV);
  }
);

onMounted(() => {
  setupEchart(props.echartDatas);
});

function setupEchart(echartDatas) {
  if (!hyEchart) {
    hyEchart = useEchart(divRef.value);
  }
  let option = getOption(echartDatas);
  hyEchart.setOption(option);
}

function getOption(echartDatas = []) {
  let categoryData = echartDatas.map((item) => {
    return item.name;
  });
  let bardata = echartDatas.map((item) => {
    return item.value;
  });

  let option = {
    grid: {
      left: "5%",
      right: "5%",
      top: "30%",
      bottom: "5%",
      containLabel: true, // grid 区域是否包含坐标轴的刻度标签
    },
    tooltip: {},
    xAxis: {// x轴
      axisLine: {// 坐标轴轴线相关设置。
        show: true,// 是否显示
        lineStyle: {// 轴线样式
          color: "#42A4FF",// 颜色
        },
      },
      axisTick: {// 坐标轴刻度相关设置
        show: false,
      },
      axisLabel: {//axisLabel 坐标轴刻度标签的相关设置。
        color: "white",
      },

      data: categoryData,
    },
    yAxis: {
      name: "个",
      nameTextStyle: {// 坐标轴名称的文字样式。
        color: "white",
        fontSize: 13,
      },
      axisLine: {// 坐标轴轴线相关设置。
        show: true,
        lineStyle: {
          color: "#42A4FF",
        },
      },
      axisTick: {
        show: false,
      },
      splitLine: {
        show: true,
        lineStyle: {
          color: "#42A4FF",
        },
      },
      axisLabel: {
        color: "white",
      },
    },
    series: [
      {
        name: "销量",
        type: "bar",
        barWidth: 17,
        itemStyle: {
          color: {
            type: "linear",
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              {
                offset: 0,
                color: "#01B1FF", // 0% 处的颜色
              },
              {
                offset: 1,
                color: "#033BFF", // 100% 处的颜色
              },
            ],
            global: false, // 缺省为 false
          },
        },
        data: bardata,
      },
    ],
  };
  return option;
}
</script>

<style scoped></style>
