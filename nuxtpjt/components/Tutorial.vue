<template>
  <div>
    <LabelButton label="클릭하면 조건부 렌더링 텍스트 나옴" />
    <div class="input-wrap">
      <input type="text" :value="message1" @input="handleInput" />
    </div>
    <p>{{ message1 }} 입니다.</p>
    <ul class="cont-list">
      <li v-for="(item, idx) in dataList" :key="idx" class="cont-list__item">
        <div>
          <div class="thumbnail">
            <img :src="dataList[idx].image" />
          </div>
          <div class="des">
            <h4 class="title">
              <button @click="openModal">{{ dataList[idx].title }}</button>
            </h4>
            <p class="price">{{ dataList[idx].price + '원' }}</p>
            <div class="btn-wrap">
              <button class="btn-basic" @click="increaseReportCount(idx)">
                허위매물신고
              </button>
              <span>신고수 : {{ 신고수[idx] }}</span>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import data from '../assets/data'
import LabelButton from './LabelButton.vue'

export default {
  name: 'NuxtTutorial',
  props: {
    modalOpen: Boolean,
  },
  components: {
    LabelButton,
  },
  data() {
    return {
      message1: '',
      dataList: data,
      신고수: [0, 0, 0],
      // products: ['역삼동', '천호동', '마포구'].map((item) => item + ' 원룸'),
      // price: [60, 70, 30].map((item) => item + '만원'),
      // images: ['room0.jpg', 'room1.jpg', 'room2.jpg'], // 이미지 파일 이름 배열
    }
  },
  methods: {
    handleInput(event) {
      this.message1 = event.target.value
    },
    increaseReportCount(index) {
      this.$set(this.신고수, index, this.신고수[index] + 1)
    },
    // getImageSrc(index) {
    //   return require(`../assets/images/${this.images[index]}`)
    // },
    openModal() {
      this.$emit('openModal')
    },
  },
}
</script>
