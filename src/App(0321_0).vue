<template>
  <div class="border shadow-[inset_0_0_15px_#86efac]">
    <div class="bg-white p-10 mb-5 shadow-[inset_0_0_15px_red]">
      <h3>퍼센트 비율 값 계산</h3>
      <div>
        <!-- change 이벤트 (input 요소에 한해서만 사용 가능) -->
        <input 
          @input="bindNumber" 
          @change="UpdateCalc" 
          v-model="calc1" 
          type="text" class="border rounded p-2 text-right" placeholder="전체값 100"> <span class="mr-2">의</span>
        <input 
          v-model="calc2" 
          type="text" class="border rounded p-2 text-right" placeholder="비율 값 20"> <span class="mr-2">%의 값은</span>
        <input 
          :value="ResultCalcA" readonly 
          type="text" class="border rounded p-2 text-right" placeholder="일부값 20"> <span class="mr-2">입니다</span>
      </div>
  </div>
  </div>
  <div class="border ">
      <div class="bg-white p-10 mb-5">
        <h3>일정 값 비율 계산</h3>
        <div>
          <input 
            v-model="calc3" 
            type="text" class="border rounded p-2 text-right" placeholder="전체 값"> <span class="mr-2">의</span>
          <input 
            v-model="calc4" 
            type="text" class="border rounded p-2 text-right" placeholder="일부 값"> <span class="mr-2">의 값은</span>
          <input 
            :value="ResultCalcB" readonly 
            type="text" class="border rounded p-2 text-right" placeholder="비율 값"> <span class="mr-2">% 입니다</span>
            <!-- 소수점 제어 -->
            <span class="text-xs ml-4">소수점 자리</span>
            <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
            <select v-model="sosujum" class="text-xs">
              <option v-for="e in 5" :value="e" :key="e">{{ e }}</option>
            </select>
        </div>
      </div>
  </div>
  <div class="border">
        <div class="bg-white p-10 mb-5">
          <h3>기준 > 변경값 비율 계산</h3>
          <input 
              v-model="calc5" 
              type="text" class="border rounded p-2 text-right" placeholder="전체 값"> <span class="mr-2">이</span>
            <input 
              v-model="calc6" 
              type="text" class="border rounded p-2 text-right" placeholder="일부 값"> <span class="mr-2">으로 값이 변경되면</span>
            <input 
              :value="ResultCalcC" readonly 
              type="text" class="border rounded p-2 text-right" placeholder="비율 변경 값"> <span class="mr-2">입니다</span>
        </div>
  </div>
  <div class="border">
    <div class="bg-white p-10 mb-5 flex items-center flex-wrap">
      <h3 class="basis-full">기존 > 변경값 비율 계산</h3>
      <div>
          <input 
            v-model="calc7" 
            type="text" class="border rounded p-2 text-right" placeholder="전체 값"> <span class="mr-2">의</span>
          <input 
            v-model="calc8" 
            type="text" class="border rounded p-2 text-right" placeholder="전체 값"> <span class="mr-2">%변경 될 시</span>
      </div>
      <div class="flex flex-col ml-10">
        <div>
          <span class="mr-2">증가값</span>
          <input 
            :value="ResultCalcD" readonly 
            type="text" class="border rounded p-2 text-right" placeholder="비율 변경 값"> <span class="mr-2">입니다</span>
        </div>
        <div class="mt-5">
          <span class="mr-2">감소값</span>
          <input 
            :value="ResultCalcE" readonly 
            type="text" class="border rounded p-2 text-right" placeholder="비율 변경 값"> <span class="mr-2">입니다</span>
        </div>
      </div>
    </div>
  </div>

</template>

<script>


export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      calc1: '',
      calc2: '',
      calc3: '',
      calc4: '',
      calc5: '',
      calc6: '',
      calc7: '',
      calc8: '',
      sosujum: '2',
    }
  },
  methods: {
    UpdateCalc(){
      console.log("11")
    },
    bindNumber(e){
      this.calc1 = e.target.value;
    },
    // 자리수 표현 함수
    hundredDigit(number){
      return number.replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",");
    },
    // 자리수 , 제거 함수
    deletDigit(commaString){
      return commaString.replace(",", "");
    }
  },
  computed: {
    ResultCalcA() {
      // a b 계산할때는 ,만 제거 하기 위해
      let a = this.deletDigit(this.calc1);
      let b = this.deletDigit(this.calc2);
      return b === '' || a === '' ? '' : this.hundredDigit((Number(a) * Number(b/100)).toFixed(this.sosujum))
    },
    ResultCalcB() {
      return this.calc4 === '' || this.calc3 === '' ? '' : ((100 * Number(this.calc4)) / Number(this.calc3)).toFixed(this.sosujum)
    },
    ResultCalcC() {
      let a = ( this.calc6 - this.calc5 ) / this.calc5 * 100;
      return this.calc6 === '' || this.calc5 === '' ? '' : a > 0 ? a.toFixed(2)+"% 증가" : a.toFixed(this.sosujum) +"% 감소"
    },
    ResultCalcD() {
      return this.calc7 === '' || this.calc8 === '' ? '' : (Number(this.calc7)  + Number(this.calc7) * Number(this.calc8/100)).toFixed(this.sosujum)
    },
    ResultCalcE() {
      return this.calc7 === '' || this.calc8 === '' ? '' : (Number(this.calc7) - Number(this.calc7) * Number(this.calc8 / 100)).toFixed(this.sosujum)
    }

  },
  // 감시 .replace(/[^0-9]/g, '') -> 숫자만 입력 가능하도록 함
  // 숫자 자리 수 표현하고 싶어서 .replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",") 를 넣었는데 문제는 문자열로 인식해서 결과값이 NaN 나옴 -> computed ResultCalcA 에서 calc1 뒤에 .replace(/[^0-9]/g, '') 를 붙여주면 계산하는 할때 다시 문자에서 숫자로 바뀌기 때문에 결과가 나오긴 함
  watch: {
    calc1: function() {
      // return this.calc1 = this.calc1.replace(/[^0-9]/g, '').replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",")
      // 위 코드 hundredDigit 함수 만들어서 간소화
      return this.calc1 = this.hundredDigit(this.calc1.replace(/[^0-9]/g, ''))
    },
    calc2: function () {
      return this.calc2 = this.hundredDigit(this.calc2.replace(/[^0-9]/g, ''))
    }
  },
  }
</script>

<style>

</style>
