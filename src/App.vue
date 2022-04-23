<script setup lang="ts">
import { World, Model, ThirdPersonCamera, Keyboard, types } from "lingo3d-vue";
import { ref } from "vue";

const pose = ref<string>("idle");
const person = ref<types.Model>()

const handleKeyPress = (key: string) => {
  if(['w','s','a','d'].includes(key)){
    pose.value = 'walking'
  }

  if(key === 'w'){
    person.value?.moveForward(-5)
  }
  if(key === 's'){
    person.value?.moveForward(5)
  }
  if(key === 'a'){
    person.value?.moveRight(5)
  }
  if(key === 'd'){
    person.value?.moveRight(-5)
  }
};

const handleKeyUp = (key: string) => {
  if(key === 'w')
    pose.value = 'idle'
};
</script>

<template>
  <World default-light="studio" :default-light-scale="3" logarithmic-depth>
    <Model src="cinema.glb" :scale="10" physics="map" :roughness-factor="0.97"/>
    <ThirdPersonCamera active mouse-control :inner-z="200">
      <Model
        src="person.fbx"
        ref="person"
        physics="character"
        :animations="{ idle: 'idle.fbx', walking: 'Walking.fbx' }"
        :animation="pose"
        pbr
      />
    </ThirdPersonCamera>
    <Keyboard @key-press="handleKeyPress" @key-up="handleKeyUp" />
  </World>
</template>
