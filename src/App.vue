<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from "./components/HelloWorld.vue";
import agents from "./agents.json";
import maps from "./maps.json";
import { ref } from "vue";
var agent_index = ref(0);
var map_index = ref(0);
var show_agent = ref(true);
var show_text = ref(true);
var victory = ref(false);
  console.log(victory);

function switchagent(index) {
  return agents[index];
}

function asurl(index) {
  return "url(" + agents[index].path + ")";
}
function saveToLocal() {
  localStorage.setItem("agent_index", agent_index.value);
  localStorage.setItem("map_index", map_index.value);
  localStorage.setItem("victory", victory.value);
  //console.log(victory);
}

function readFromLocal() {
  if (localStorage.length > 0) {
    victory.value = localStorage.getItem("victory");
    agent_index.value = localStorage.getItem("agent_index");
    map_index.value = localStorage.getItem("map_index");
    show_agent.value = true
    show_text.value = true
  }else{
    }

}
    readFromLocal();
</script>

<template>
  <div class="maincontainer">
    <div class="container">
      <div
        v-show="show_agent"
        class="overlay"
        :style="{ MaskImage: asurl(agent_index) }"
        :class="{ victory: victory }"
      ></div>
      <img
        :src="switchagent(agent_index).path"
        class="agent saturation"
        v-show="show_agent"
      />
      <!--div class="channelname">
        wynywynyr
      </div!-->
      <div class="textcontainer" v-show="show_text">
        <div class="border"></div>
        <div v-if="victory" class="text victory">victory</div>
        <div class="text defeated" v-else>defeated</div>

        <div class="box box01"></div>
        <div class="box box02"></div>

        <div class="box box03"></div>
        <div class="box box04"></div>
      </div>
      <img src="./assets/EndScreen.png" class="endscreen" />
      <img :src="maps[map_index].path" class="map" />
    </div>
  </div>
  <div class="console">
    <button @click="(show_agent = !show_agent), saveToLocal()">
      <span v-if="show_agent">Hide</span>
      <span v-else>Show</span>
      Agent
    </button>
    <button @click="show_text = !show_text">
      <span v-if="show_text">Hide</span>
      <span v-else>Show</span>
      Text
    </button>
    <button @click="victory = !victory">
      current
      <span v-if="victory">WIN</span>
      <span v-else>LOSE</span>
    </button>
    <br />
    <button
      v-for="(i, index) in agents"
      @click="agent_index = index"
      style="text-transform: capitalize"
    >
      {{ i.name }}
    </button>
    <br />
    <button alt="" v-for="(j, jindex) in maps" @click="map_index = jindex">
      {{ j.name }}
    </button>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Anton&display=swap");
#app {
  font-family: Anton, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
::-webkit-scrollbar {
  display: none;
}
</style>

<style scoped>
.console {
  z-index: 99;
  position: absolute;
  top: 720px;
}
.maincontainer {
  position: absolute;
  width: 100%;
  max-width: 1280px;
}
.container {
  position: relative;
  top: -53px;
  left: 0;
  margin-top: -7px;
}
img.map {
  width: 100%;
  max-width: 1280px;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 100%;
  z-index: 10;
  mask-image: url("./assets/Maskgroup.png");
  -webkit-mask-image: url("./assets/Maskgroup.png");
  -webkit-mask-mode: alpha;
  mask-mode: alpha;
  -webkit-mask-size: 100%;
  mask-size: 10%;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
}
.channelname{
  position: absolute;
    top: 18%;
    left: -3%;
    font-size: 36px;
  z-index: 98;
  text-transform: uppercase;
  transform:rotate(90deg);
  color: #e4e0da;
  content: none;
  text-anchor: start;
}
img.endscreen {
  position: absolute;
  top: 0;
  left: 0;
  opacity: 100%;
  width: 100%;
  z-index: 10;
  max-width: 1280px;
}
div.overlay {
  position: absolute;
  top: 0%;
  left: 3%;
  opacity: 40%;
  height: 720px;
  width: 480px;
  z-index: 90;
  background-image: linear-gradient(transparent, transparent);
  -webkit-mask-size: 100%;
  mask-size: 10%;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
  transition: background-image 2s;
}
div.overlay.victory {
  background-image: linear-gradient(45deg, #ff4654 0%, #0f1822 150%);
  animation: overlay-slide 0.25s linear;
}
img.agent {
  position: absolute;
  object-position: top;
  left: 3%;
  z-index: 50;
  height: 720px;
  width: 480px;
  opacity: 100%;
  object-fit: cover;
  animation: overlay-slide 0.3s linear;
}
@keyframes overlay-slide {
  0% {
    opacity: 0%;
  }
  100% {
    opacity: 40%;
  }
}

.textcontainer {
  position: relative;
  text-align: end;
  top: 0;
  left: 0;
  z-index: 50;
  width: 1280px;
  height: 720px;
  background-color: #ff465400;
  animation: opacity-ani 1.25s ease;
}

.textcontainer .text {
  position: absolute;
  font-family: "Anton", sans-serif;
  text-transform: uppercase;
  animation: pop-up-text 0.7s ease;
}
.text.victory {
  font-size: 185px;
  top: calc(29% + 17px);
  left: calc(42% + 63px);
  color: #e4e0da;
}
.text.defeated {
  font-size: 163.89px;
  top: calc(29% + 33px);
  left: calc(42% + 51px);
  color: #6e6e6e;
}
.border {
  position: absolute;
  top: 29%;
  right: 5%;
  height: 305px;
  width: 676px;
  background-color: #0f1822f0;
}
.box {
  position: absolute;
  height: 10px;
  width: 10px;
  background-color: #ff4654;
  animation: pop-up-box 0.5s ease;
}
.box01 {
  top: 29%;
  right: 5%;
}
.box02 {
  top: 29%;
  right: 57%;
}
.box03 {
  top: 70%;
  right: 5%;
}
.box04 {
  top: 70%;
  right: 57%;
}
.saturation {
  filter: saturate(0);
  animation: overlay-slide 0.3s linear;
  
}

@keyframes pop-up-text {
  0% {
    opacity: 0%;
    color: #ff4654;
    top: 35%;
  }
  100% {
    opacity: 100%;
  }
}
@keyframes pop-up-box {
  0% {
    opacity: 0%;
    top: 49%;
  }
  100% {
    opacity: 100%;
  }
}

@keyframes opacity-ani {
  0% {
    opacity: 0%;
  }
  100% {
    opacity: 100%;
  }
}
</style>
