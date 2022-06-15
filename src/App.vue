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
            <div class="views">
                <div class="vies-menu" @drop="drop" @dragover="dragover">
                    <div class="source-item" draggable="true" id="id-15">
                        15
                    </div>
                    <div
                        class="source-item draw-view"
                        draggable="true"
                        id="id-16"
                        style="position: absolute; left: 300px; top: 700px"
                        @drag="viewdrop"
                    >
                        16
                    </div>
                </div>

                <svg
                    id="svgs"
                    style="
                        position: absolute;
                        top: 0;
                        width: 100%;
                        height: 100%;
                    "
                >
                    <defs>
                        <marker
                            id="arrow"
                            markerUnits="strokeWidth"
                            markerWidth="12"
                            markerHeight="12"
                            viewBox="0 0 12 12"
                            refX="6"
                            refY="6"
                            orient="auto"
                        >
                            <path
                                xmlns="http://www.w3.org/2000/svg"
                                d="M2,2 L10,6 L2,10 L6,6 L2,2"
                                style="fill: #000000"
                                id="path"
                            />
                        </marker>
                    </defs>
                    <line
                        x1="190"
                        y1="115"
                        x2="490"
                        y2="715"
                        stroke="#000"
                        stroke-width="2"
                        marker-end="url(#arrow)"
                        stroke-dasharray="10,10"
                    ></line>
                </svg>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
const key = ref('')

onMounted(() => {
  const el1 = document.getElementById('id-15')
  const el2 = document.getElementById('id-16')
  const line = document.createElement('path')
  //   const path = document.getElementById('path')
  //   const views = document.querySelector('.views')
  const svgs = document.getElementById('svgs')
  console.log(line)
  console.log(key.value)
  const getPos = (element) => {
    const top = element.offsetTop
    const left = element.offsetLeft
    const width = element.offsetWidth
    const height = element.offsetHeight
    return { top, left, width, height }
  }
  const drawline = (el) => {
    const pos1 = getPos(el1)
    const pos2 = getPos(el2)
    const start = PosSet(pos1, pos2).start
    const end = PosSet(pos1, pos2).end
    console.log(start, end)
    svgs.appendChild(line)
  }
  const PosSet = function (p1, p2) {
    let x1 = ''
    let y1 = ''
    let x2 = ''
    let y2 = ''
    if (p2.top < p1.top) {
      x1 = p1.left + p1.width / 2
      y1 = p1.top
      y2 = p2.top + p2.height
      if (p2.left < p1.left) {
        x2 = p2.left + p2.width
      }
    } else {
      x1 = p1.left + p1.width / 2
      y1 = p1.top + p1.height
      x2 = p2.left + p2.width / 2
      y2 = p2.top
    }
    return {
      start: { x: x1, y: y1 },
      end: { x: x2, y: y2 }
    }
  }
  drawline()
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
  if (e.dataTransfer.effectAllowed === 'all') {
    e.preventDefault()
  } else {
    console.log(e.dataTransfer)
    //   const side = document.querySelector('.source')
    e.preventDefault()
    const options = [e.offsetX, e.offsetY] // 扣除元素本身寬高把元素定位在數標中心點不然會以元素左上角做定位為智會錯誤
    const data = e.dataTransfer.getData('text')
    const original = document.getElementById(data)
    const cloneData = original.cloneNode(true)
    cloneData.draggable = false
    cloneData.classList.add('draw-view')
    cloneData.setAttribute(
      'style',
      'position: absolute; top: ' +
        options[1] +
        'px; left:' +
        options[0] +
        'px; margin:0;'
    )
    console.log(data, origin, options)
    e.target.appendChild(cloneData)
  }
}

const dragover = (e) => {
  e.preventDefault()
}
const viewdrop = (e) => {
  console.log(e)
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
        position: relative;
        flex: 1 1 80%;
        background: rgb(235, 233, 233);
        .vies-menu {
            width: 100%;
            height: 100%;
            position: absolute;
            z-index: 1060;
            .source-item {
                width: 350px;
                height: 100px;
                background: skyblue;
                margin: 15px;
            }
        }
    }
}
</style>
