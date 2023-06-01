<template>
    <div class="setCountTimer">
      <h1 class="title">집에 가고 싶다</h1>
      <div class="setCount">
        <p>{{ currentDate }}</p>
        <label for="targetTime">나의 퇴근 시간</label>
        <input type="time" id="targetTime" v-model="targetTime" class="setTimer" @change="updateTargetTime"/>
        <p class="statusMsg" :class="errorMessageClass" v-if="errorMessage !== ''">{{ errorMessage }}</p>
      </div>
      <p v-if="timer !== ''">퇴근까지 {{ timer }} 남았습니다.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        targetTime: '', // 사용자가 선택한 시간을 문자열로 저장할 변수
        targetDate: null, // 항상 오늘 날짜로 설정되는 Date 객체
        timer: '', // 남은 시간을 표시할 변수
        currentDate: '', // 오늘 날짜를 표시할 변수
        errorMessage: '', // 에러 메시지를 표시할 변수
        errorMessageClass:''
      };
    },
    created() {
      this.targetDate = this.getTodayDate(); // 항상 오늘 날짜로 설정
      this.currentDate = this.getCurrentDate(); // 오늘 날짜를 가져와서 변수에 저장
    },
    methods: {
        getCurrentDate() {
            const currentDate = new Date();
            const options = { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' };
            return currentDate.toLocaleDateString('ko-KR', options);
        },
      calculateTime() {
        if (!this.targetDate) return; // targetDate가 null인 경우 계산하지 않음
  
        const currentDate = new Date(); // 현재 날짜와 시간 가져오기
        const remainingTime = this.targetDate - currentDate; // 남은 시간 계산 (밀리초 단위)
        const hours = Math.floor((remainingTime % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)); // 남은 시간을 시간 단위로 계산
        const minutes = Math.floor((remainingTime % (1000 * 60 * 60)) / (1000 * 60)); // 남은 시간을 분 단위로 계산
        const seconds = Math.floor((remainingTime % (1000 * 60)) / 1000); // 남은 시간을 초 단위로 계산
  
        this.timer = `${hours}시간 ${minutes}분 ${seconds}초`; // 결과를 timer 변수에 저장하여 화면에 표시
      },
      updateTargetTime() {

        if (this.targetTime === '') {
            // 사용자가 시간을 선택하지 않았거나 빈 값인 경우
            this.errorMessage = '시간을 선택해주세요.';
            clearInterval(this.intervalId); // setInterval() 함수 중지
            return;
        }
        const currentTime = new Date();
        const selectedDate = new Date(
        currentTime.getFullYear(),
        currentTime.getMonth(),
        currentTime.getDate(),
        this.targetTime.split(':')[0],
        this.targetTime.split(':')[1]
    );

    if (selectedDate > currentTime) {
      // 선택한 시간이 현재 시간보다 이후인 경우
      this.targetDate = selectedDate;
      this.errorMessage = '';
      this.calculateTime(); // 남은 시간을 다시 계산하여 업데이트

      // 1초마다 남은 시간을 업데이트하기 위해 setInterval() 함수 호출
      this.intervalId = setInterval(() => {
        this.calculateTime();
      }, 1000);
    } else {
      // 선택한 시간이 현재 시간이거나 이전인 경우
      this.targetTime = '';
      this.targetDate = null;
      this.errorMessage = '현재 시간보다 이후 시간을 선택해주세요.';

      // 초기 진입 상태이므로 setInterval() 함수 호출하지 않음
      clearInterval(this.intervalId);
        }
    },
      getTodayDate() {
        const currentDate = new Date();
        return new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate());
      },
    },
  };
  </script>
  