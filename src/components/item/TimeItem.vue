<template>
  <div id="app">
    <!-- 时钟的主体 -->
    <div
      v-drag
      :class="{ clock_dark: isdark, clock: !isdark }"
      :style="{ borderRadius: radius }"
    >
      <!-- 时钟的时针,是透明的，时针主要靠cover层实现 -->
      <div id="hour_pointer" :style="{ transform: hourDeg }">
        <!-- 时钟的时针是个长的东西,hour_cover是时针的样式颜色 也就是能看到的时针 -->
        <div class="hour_cover"></div>
      </div>
      <!-- 下面就是分针秒针 实现方式一样 -->
      <div id="min_pointer" :style="{ transform: minDeg }">
        <div class="min_cover"></div>
      </div>
      <div id="sec_pointer" :style="{ transform: secDeg }">
        <div class="sec_cover"></div>
      </div>
      <!-- 这是中间的遮盖点 -->
      <div id="dot" />
      <!-- 这是数字时间 -->
      <div id="digclock" :style="{ backgroundColor: bgdark }">
        {{ hour }}:{{ min }}:{{ sec }}
      </div>
      <!-- 下面的4个span是时钟上的4个数字,要不要都行 -->
      <span
        v-for="number in 12"
        :key="number"
        :class="{ number_dark: isdark, number: !isdark }"
        :style="getPositionStyle(number)"
      >
        {{ number }}
      </span>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";

const hourDeg = ref("");
const minDeg = ref("");
const secDeg = ref("");
const hour = ref("12");
const min = ref("00");
const sec = ref("00");
const radius = ref("200px");
const isdark = ref(false);
const bgdark = ref("");

// const drag = (el) => {
//   const onMouseDown = (e) => {
//     const disx = e.pageX - el.offsetLeft;
//     const disy = e.pageY - el.offsetTop;
//     const onMouseMove = (e) => {
//       el.style.left = e.pageX - disx + "px";
//       el.style.top = e.pageY - disy + "px";
//     };
//     const onMouseUp = () => {
//       document.onmousemove = null;
//       document.onmouseup = null;
//     };
//
//     document.onmousemove = onMouseMove;
//     document.onmouseup = onMouseUp;
//   };
//
//   el.addEventListener("mousedown", onMouseDown);
// };

const changeTime = () => {
  const date = new Date();
  const minutes = date.getMinutes();
  const hours = date.getHours();
  const seconds = date.getSeconds();
  secDeg.value = `rotate(${seconds * 6}deg)`;
  minDeg.value = `rotate(${minutes * 6}deg)`;
  hourDeg.value = `rotate(${hours * 30}deg)`;
  hour.value = hours < 10 ? "0" + hours : hours;
  min.value = minutes < 10 ? "0" + minutes : minutes;
  sec.value = seconds < 10 ? "0" + seconds : seconds;
};

const getPositionStyle = (number) => {
  const position = calculatePosition(number);
  return {
    position: "absolute",
    marginTop: `${position.y}px`,
    marginLeft: `${position.x}px`,
  };
};

const calculatePosition = (number) => {
  const radius = 190; // 半径
  const angle = ((number - 3) * 30 * Math.PI) / 180; // 角度，每个数字之间相差30度
  const x = radius * Math.cos(angle);
  const y = radius * Math.sin(angle);
  return { x, y };
};

onMounted(() => {
  changeTime();
  setInterval(changeTime, 1000);
});
</script>

<style>
.number {
  text-align: center;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  user-select: none;
}

.number_dark {
  text-align: center;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  user-select: none;
  color: #fafafa;
}

.clock {
  width: 200px;
  height: 200px;
  display: flex;
  position: absolute;
  right: 150px;
  top: 160px;
  background-color: rgba(170, 170, 250, 0.34);
  box-shadow: 3px 3px 5px #dddddd;
  padding: 10px;
  justify-content: center;
  align-items: center;
  transition: all 200ms linear;
}

.clock_dark {
  width: 200px;
  height: 200px;
  display: flex;
  position: absolute;
  left: 0;
  top: 0;
  background-color: #333333;
  box-shadow: 3px 3px 5px #333333;
  padding: 10px;
  justify-content: center;
  align-items: center;
  transition: all 200ms linear;
}

#clock > div {
  display: flex;
  flex-direction: column;
  user-select: none;
}

#hour_pointer {
  background-color: transparent;
  position: absolute;
  height: 110px;
  width: 4px;
  z-index: 1;
}

.hour_cover {
  width: 100%;
  height: 50%;
  background-color: darkred;
}

#min_pointer {
  background-color: transparent;
  position: absolute;
  height: 150px;
  width: 3px;
  z-index: 2;
}

.min_cover {
  width: 100%;
  height: 50%;
  background-color: dimgray;
}

#sec_pointer {
  position: absolute;
  background-color: transparent;
  height: 190px;
  width: 2px;
  z-index: 3;
}

.sec_cover {
  width: 100%;
  height: 50%;
  background-color: dodgerblue;
}

#dot {
  position: absolute;
  background-color: #aaaaaa;
  height: 15px;
  width: 15px;
  border-radius: 10px;
  /* box-shadow: 3px 3px 5px #dddddd; */
  z-index: 4;
  transform: rotate(120deg);
}

#digclock {
  width: 100px;
  height: 30px;
  background-color: #cccccc;
  /* box-shadow: 2px 2px 3px #eeeeee; */
  color: #ffffff;
  border-radius: 5px;
  margin-top: 280px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  white-space: nowrap;
  font-size: large;
  user-select: none;
  transition: background-color 200ms linear;
}
</style>
