<template>
    <div>
      <h1>디데이 카운트</h1>
      <div>
        <label for="targetDate">목표 날짜</label>
        <input type="date" id="targetDate" v-model="targetDate" @change="calculateRemainingDays" />
        <p v-if="remainingDays !== null">D-{{ remainingDays }}</p>
        <p class="statusMsg" :class="errorMessageClass" v-if="errorMessage !== ''">{{ errorMessage }}</p>
        <label for="excludeWeekends">주말 제외</label>
        <input type="checkbox" id="excludeWeekends" v-model="excludeWeekends" @change="calculateRemainingDaysWithoutWeekends" />
        <p v-if="remainingDaysWithoutWeekends !== null">D-{{ remainingDaysWithoutWeekends }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        targetDate: '', // 사용자가 선택한 목표 날짜
        remainingDays: null, // 남은 일수를 저장할 변수
        remainingDaysWithoutWeekends: null, // 주말을 제외한 남은 일수를 저장할 변수
        errorMessage: '',
      };
    },
    watch: {
        targetDate(newValue, oldValue) {
        // 사용자가 새로운 날짜를 선택할 때마다 주말 제외값 초기화
        if (newValue !== oldValue) {
            this.remainingDaysWithoutWeekends = null;
        }
        },
    },
    methods: {
      calculateRemainingDays() {
        const currentDate = new Date(); // 현재 날짜와 시간 가져오기
        const targetDateTime = new Date(this.targetDate).setHours(0, 0, 0, 0); // 목표 날짜의 00:00:00 시간으로 설정
  
        // 현재 날짜와 목표 날짜 사이의 시간 차이 계산
        const timeDiff = targetDateTime - currentDate.getTime();
  
        // 차이를 일 단위로 변환하여 남은 일수 계산
        const remainingDays = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
  
        if (targetDateTime > currentDate) {
          this.errorMessage = '';
          this.remainingDays = remainingDays; // 결과를 변수에 저장하여 화면에 표시
        } else {
          this.targetDate = '';
          this.errorMessage = '현재 날짜보다 이전 날짜를 선택해주세요.';
        }
      },
      calculateRemainingDaysWithoutWeekends() {
            const currentDate = new Date();
            const targetDateTime = new Date(this.targetDate).setHours(0, 0, 0, 0);

            let remainingDaysWithoutWeekends = 0;

            if (targetDateTime > currentDate) {
                let startDate = new Date(currentDate); // 시작일을 현재 날짜로 설정
                const endDate = new Date(targetDateTime); // 종료일을 목표 날짜로 설정

                while (startDate <= endDate) {
                if (!this.excludeWeekends || (startDate.getDay() !== 0 && startDate.getDay() !== 6)) {
                    // 주말 제외 체크박스가 선택되어 있지 않거나 주말이 아닌 경우에만 일수를 증가
                    remainingDaysWithoutWeekends++;
                }
                startDate = new Date(startDate.getTime() + 24 * 60 * 60 * 1000); // 다음 날짜로 업데이트
                }

                this.remainingDaysWithoutWeekends = remainingDaysWithoutWeekends; // 결과를 변수에 저장하여 화면에 표시
            } else {
                this.remainingDaysWithoutWeekends = null;
                this.errorMessage = '현재 날짜보다 이후 날짜를 선택해주세요.';
            }
            },
  },
};
  </script>
  