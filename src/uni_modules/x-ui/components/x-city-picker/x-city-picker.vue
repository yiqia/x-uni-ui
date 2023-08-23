<template>
  <view class="x-city-picker">
    <picker-view
      :indicator-style="indicatorStyle"
      :immediate-change="immediateChange"
      :indicator-class="indicatorClass"
      :mask-style="maskStyle"
      :mask-top-style="maskTopStyle"
      :value="pickerValue"
      @change="bindChange"
      @pickstart="(e) => emit('pickstart', e)"
      @pickend="(e) => emit('pickend', e)"
      class="x-city-picker-view"
    >
      <picker-view-column>
        <view
          class="x-city-picker-view-column"
          v-for="item in provinceData"
          :key="item.value"
          >{{ item.label }}</view
        >
      </picker-view-column>
      <picker-view-column>
        <view
          class="x-city-picker-view-column"
          v-for="item in cityOptions"
          :key="item.value"
          >{{ item.label }}</view
        >
      </picker-view-column>
      <picker-view-column>
        <view
          class="x-city-picker-view-column"
          v-for="item in areaOptions"
          :key="item.value"
          >{{ item.label }}</view
        >
      </picker-view-column>
    </picker-view>
  </view>
</template>
<script lang="ts" setup>
import { ref, onMounted, PropType, watch } from "vue";
import { CityPickerType } from "./constants/type";
import { provinceData } from "./constants/province";
import { cityData } from "./constants/city";
import { areaData } from "./constants/area";

const emit = defineEmits(["change", "pickstart", "pickend"]);
const props = defineProps({
  indicatorStyle: {
    type: String,
    default: "height: 50px;",
  },
  indicatorClass: {
    type: String,
    default: "",
  },
  maskStyle: {
    type: String,
    default: "",
  },
  maskTopStyle: {
    type: String,
    default: "",
  },
  maskBottomStyle: {
    type: String,
    default: "",
  },
  maskClass: {
    type: String,
    default: "",
  },
  immediateChange: {
    type: Boolean,
    default: true,
  },
  value: {
    type: Array as PropType<number[]>,
    default: null,
  },
  joinStr: {
    type: String,
    default: ",",
  },
});
// 选择的下标值
const pickerValue = ref<number[]>([0, 0, 0]);
const cityOptions = ref<CityPickerType[]>(cityData[0]);
const areaOptions = ref<CityPickerType[]>(areaData[0][0]);

onMounted(() => {
  if (props.value) {
    pickerValue.value = [...props.value];
  }
});

// 监听传入的value值 改变后更新选项值
watch(
  () => props.value,
  (val) => {
    if (val && val.length > 0) {
      pickerValue.value = [...val];
    }
  }
);

const bindChange = (e) => {
  const val = e.detail.value;
  let [proIndex = 0, cityIndex = 0, areaIndex = 0] = val;
  // 选择改变后下级选项重置为0
  if (proIndex !== pickerValue.value[0]) {
    cityIndex = 0;
    areaIndex = 0;
  } else if (cityIndex !== pickerValue.value[1]) {
    areaIndex = 0;
  }
  pickerValue.value = [proIndex, cityIndex, areaIndex];

  cityOptions.value = cityData[proIndex];
  areaOptions.value = areaData[proIndex][cityIndex];
  const province = provinceData[proIndex];
  const city = cityData[proIndex][cityIndex];
  const area = areaData[proIndex][cityIndex][areaIndex];

  emit("change", {
    label: [province.label, city.label, area.label],
    value: [province.value, city.value, area.value],
    text: [province.label, city.label, area.label].join(props.joinStr),
  });
};
</script>

<style scoped lang="scss">
.x-city-picker {
  &-view {
    width: 750rpx;
    height: 600rpx;
    margin-top: 20rpx;

    &-column {
      flex: 1;
      text-align: center;
      line-height: 100rpx;
      font-size: 28rpx;
      color: $x-main-color;
    }
  }
}
</style>
