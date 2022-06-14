<template>
    <div>
        <div class="outter">
            <div class="source">
                <div
                    class="source-item"
                    draggable="true"
                    v-for="item in 3"
                    :key="item"
                    :id="`id-${item}`"
                    @dragstart="dragstart"
                >
                    {{ item }}
                </div>
            </div>
            <div class="views" @drop="drop" @dragover="dragover"></div>
        </div>
    </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
const key = ref('')
onMounted(() => {
  console.log(key.value)
  // const soucre = document.querySelectorAll('.source-item')
  // soucre.forEach((block) => {
  //   block.addEventListener('dragstart', dragstart)
  // })
  // const target = document.querySelector('.views')
  // target.addEventListener('drop', drop)
  // target.addEventListener('dragenter', dragenter)
  // target.addEventListener('dragover', dragover)
})
const dragstart = (e) => {
  e.dataTransfer.setData('text', e.target.id)
  e.dataTransfer.effectAllowed = 'copy'
}
const drop = (e) => {
  console.log(e)
  e.preventDefault()
  const options = [e.clientX - 175, e.clientY - 50] // 扣除元素本身寬高把元素定位在數標中心點不然會以元素左上角做定位為智會錯誤
  const data = e.dataTransfer.getData('text')
  const original = document.getElementById(data)
  const cloneData = original.cloneNode(true)
  cloneData.draggable = false
  cloneData.setAttribute(
    'style',
    'position: absolute; top: ' + options[1] + 'px; left:' + options[0] + 'px;'
  )
  console.log(data, origin, options)
  e.target.appendChild(cloneData)
}

const dragover = (e) => {
  e.preventDefault()
}
</script>

<style lang="scss">
.outter {
    display: flex;
    height: 1000px;
    .source {
        flex: 1 1 20%;
        background: antiquewhite;
        .source-item {
            width: calc(100% - 30px);
            height: 100px;
            background: skyblue;
            margin: 15px;
        }
    }
    .views {
        flex: 1 1 80%;
        background: gray;
        .source-item {
            width: 350px;
            height: 100px;
            background: skyblue;
            margin: 15px;
        }
    }
}
</style>
