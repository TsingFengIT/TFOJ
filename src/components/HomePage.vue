<template>
  <div class="container1">
    <a-carousel
      :style="{
        width: '1000px',
        height: '320px',
      }"
      :auto-play="true"
      :default-current="2"
      @change="handleChange"
    >
      <a-carousel-item v-for="(item, index) in listUrl" :key="index">
        <a :href="item.url" target="_blank">
          <img
            :src="item.image"
            :style="{
              width: '100%',
            }"
        /></a>
      </a-carousel-item>
    </a-carousel>
    <TimeItem />

    <!--引入粒子特效-->
    <MdEditor
      :value="form.content"
      :handle-change="onContentChange"
      style="z-index: 999"
    />
  </div>
</template>

<script setup lang="ts">
import TimeItem from "@/components/item/TimeItem.vue";
import MdEditor from "@/components/MdEditor.vue";
import { ref } from "vue";
import MyFile from "@/components/item/MyFile.vue";

let form = ref({
  title: "",
  tags: [],
  answer: "",
  content: "",
  judgeConfig: {
    memoryLimit: 1000,
    stackLimit: 1000,
    timeLimit: 1000,
  },
  judgeCase: [
    {
      input: "",
      output: "",
    },
  ],
});
const listUrl = [
  {
    image: "https://www.haust.edu.cn/images/weixintupian_20240313102225.jpg",
    url: "https://www.haust.edu.cn/",
  },
  {
    image: "https://www.haust.edu.cn/images/1.jpg",
    url: "https://www.haust.edu.cn/",
  },
];
const handleChange = (value: string) => {
  console.log(value);
};
const onContentChange = (value: string) => {
  form.value.content = value;
};
</script>
<style>
.container1 {
  width: 1300px;
  margin: auto;
}
</style>
