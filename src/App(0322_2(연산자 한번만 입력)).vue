<template>
  <div :class="[themecolor[theme] && themecolor[theme].back]"  class="p-5 text-base md:text-xl xl:text-2xl flex justify-between">
    <h3 class="font-extrabold">계산기</h3>
    <div class="">
      <span>테마 : </span>
      <button class="mx-1 md:mx-2 xl:mx-3 font-bold text-blue-800" @click="theme = 'default'">기본</button>
      <button class="mx-1 md:mx-2 xl:mx-3 font-bold text-green-800" @click="theme = 'green'">그린</button>
      <button class="mx-1 md:mx-2 xl:mx-3 font-bold text-pink-800" @click="theme = 'pink'">핑크</button>
    </div>
  </div>

  <ul class="flex justify-center gap-3 text-center text-3xl my-10">
    <li :class="[themecolor[theme] && themecolor[theme].text, cateName === '비율계산' && 'underline'] " @click="cateName = '비율계산'">비율계산</li> 
    <li :class="[themecolor[theme] && themecolor[theme].text, cateName === '단순계산' && 'underline'] " @click="cateName = '단순계산'">단순계산</li>
  </ul>
  <div v-if="cateName === '비율계산'">
    <!-- <h3 class="text-center text-5xl my-10">비율 계산</h3> -->
    <div class="flex flex-wrap justify-center min-w-[480px] max-w-7xl mx-auto gap-1 group">
      <div class="border basis-[100%] md:basis-[48%] " :class="[themecolor[theme] && themecolor[theme].back, themecolor[theme] && themecolor[theme].text]">
        <div class="p-10" >
          <h3>비율 계산</h3>
          <div>
            <!-- change 이벤트 (input 요소에 한해서만 사용 가능) -->
            <!-- @input="bindNumber"  -->
            <input 
              @change="UpdateCalc" 
              v-model="calc1" 
              type="text" class="border rounded p-1 text-right" placeholder="전체값 100"> <span class="mr-2">의</span>
            <input 
              v-model="calc2" 
              type="text" class="border rounded p-1 text-right mt-1" placeholder="비율 값 20"> <span class="mr-2">%의 값은</span>
            <input 
              :value="ResultCalcA" readonly 
              type="text" class="border rounded p-1 text-right mt-2" placeholder="일부 값 20"> <span class="mr-2">입니다</span>
            <!-- 소수점 제어 -->
            <span class="text-xs ml-1">소수점</span>
            <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
            <select v-model="resultASosujum" class="text-xs text-black">
              <option v-for="e in 5" :value="e - 1" :key="e">{{ e - 1 }}</option>
            </select>
          </div>
      </div>
      </div>
      <div class="border basis-[100%] md:basis-[48%] " :class="[themecolor[theme] && themecolor[theme].back, themecolor[theme] && themecolor[theme].text]">
          <div class="p-10" >
            <h3>값 계산</h3>
            <div>
              <input 
                v-model="calc3" 
                type="text" class="border rounded p-1 text-right" placeholder="전체 값 100"> <span class="mr-2">의</span>
              <input 
                v-model="calc4" 
                type="text" class="border rounded p-1 text-right mt-1" placeholder="일부 값 10"> <span class="mr-2">의 값은</span>
              <input 
                :value="ResultCalcB" readonly 
                type="text" class="border rounded p-1 text-right mt-2" placeholder="비율 값 10.0"> <span class="mr-2">% 입니다</span>
                <!-- 소수점 제어 -->
                <span class="text-xs ml-1">소수점</span>
                <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
                <select v-model="resultBSosujum" class="text-xs text-black">
                  <option v-for="e in 5" :value="e - 1" :key="e">{{ e - 1 }}</option>
                </select>
            </div>
          </div>
      </div>
      <div class="border basis-[100%] md:basis-[48%] " :class="[themecolor[theme] && themecolor[theme].back, themecolor[theme] && themecolor[theme].text]">
            <div class="p-10" >
              <h3>증감 비율 계산</h3>
              <input 
                  v-model="calc5" 
                  type="text" class="border rounded p-1 text-right" placeholder="전체 값"> <span class="mr-2">이</span>
                <input 
                  v-model="calc6" 
                  type="text" class="border rounded p-1 text-right mt-1" placeholder="일부 값"> <span class="mr-2">으로 바뀌면</span>
                <input 
                  :value="ResultCalcC" readonly 
                  type="text" class="border rounded p-1 text-right mt-2" placeholder="증감 비율"> <span class="mr-2">입니다</span>
                <!-- 소수점 제어 -->
                <span class="text-xs ml-1">소수점</span>
                <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
                <select v-model="resultCSosujum" class="text-xs text-black">
                  <option v-for="e in 5" :value="e - 1" :key="e">{{ e - 1 }}</option>
                </select>
            </div>
      </div>
      <div class="border basis-[100%] md:basis-[48%] " :class="[themecolor[theme] && themecolor[theme].back, themecolor[theme] && themecolor[theme].text]">
        <div class="p-10 flex items-center flex-wrap" >
          <h3 class="basis-full">증감값 계산</h3>
          <div>
              <input 
                v-model="calc7" 
                type="text" class="border rounded p-1 text-right" placeholder="전체 값"> <span class="mr-2">의</span>
              <input 
                v-model="calc8" 
                type="text" class="border rounded p-1 text-right mt-1" placeholder="비율 값"> <span class="mr-2">%변경 될 시</span>
          </div>
          <div class="flex flex-col mt-2">
            <div>
              <span class="mr-2">증가된 값</span>
              <input 
                :value="ResultCalcD" readonly 
                type="text" class="border rounded p-1 text-right" placeholder="증가값"> <span class="mr-2">입니다</span>
                <!-- 소수점 제어 -->
                <span class="text-xs ml-1">소수점</span>
                <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
                <select v-model="resultDSosujum" class="text-xs text-black">
                  <option v-for="e in 5" :value="e - 1" :key="e">{{ e - 1 }}</option>
                </select>
            </div>
            <div class="mt-2">
              <span class="mr-2">감소된 값</span>
              <input 
                :value="ResultCalcE" readonly 
                type="text" class="border rounded p-1 text-right" placeholder="감소값"> <span class="mr-2">입니다</span>
              <!-- 소수점 제어 -->
              <span class="text-xs ml-1">소수점</span>
              <!-- 선택하는 option 값이 select 값으로 변경되고(select option의 기본 특징) v-model 을 설정함으로써 data sosujum 값도 바로 변경되도록 함 -->
              <select v-model="resultESosujum" class="text-xs text-black">
                <option v-for="e in 5" :value="e - 1" :key="e">{{ e - 1 }}</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 추가 작업 중 -->
  <div v-if="cateName === '단순계산'">
    <!-- <h3 class="text-center text-5xl my-10">간단 계산기</h3> -->
    <div class="py-5" :class="[themecolor[theme] && themecolor[theme].back]">
      <div class="calculator border border-#333] bg-[#ccc] w-[80%] md:w-[50%] lg:w-[40%] mx-auto p-1">
        <form name="forms" class="grid grid-cols-4 gap-1 grid-rows-[45px_40px_40px_40px_40px_40px]">
      <!-- input 태그에 @keydown="keyBindNumber()  
      스크립트 내에 
      keyBindNumber(){
      const key = document.forms.output.value.key;
      alert(key)
      }, 
      추가 시 키보드 입력 함수가 실행되긴 하지만 문제는 
      1) 입력 창이 활성화 된 상태에서만 키보드 입력 가능
      2) key 값 가져오기는 실패하였음 함수만 실행된 상태   -->
          <input type="text" readonly name="output" class="col-span-4 border rounded text-lg font-bold cursor-pointer text-right px-3" />
          <input @click="clearOutput" type="button" class="clear col-span-3 bg-red-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="C" />
          <input @click="bindOperator('/')" type="button" class="operator bg-orange-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="/" />
          <input @click="bindNumber(1)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="1" />
          <input @click="bindNumber(2)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="2" />
          <input @click="bindNumber(3)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="3" />
          <input @click="bindOperator('*')" type="button" class="operator bg-orange-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="*" />
          <input @click="bindNumber(4)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="4" />
          <input @click="bindNumber(5)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="5" />
          <input @click="bindNumber(6)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="6" />
          <input @click="bindOperator('+')" type="button" class="operator bg-orange-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="+" />
          <input @click="bindNumber(7)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="7" />
          <input @click="bindNumber(8)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="8" />
          <input @click="bindNumber(9)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="9" />
          <input @click="bindOperator('-')" type="button" class="operator bg-orange-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="-" />
          <input @click="bindOperator('.')" type="button" class="dot bg-green-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="." />
          <input @click="bindNumber(0)" type="button" class="bg-white text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="0" />
          <input @click="calcResult('=')" type="button" class="operator result col-span-2 bg-orange-500 text-lg font-bold cursor-pointer border border-[#333] hover:shadow-[1px_1px_2px_#333]" value="=" />
        </form>
      </div>
    </div>
  </div>


</template>

<script>
// input 태그에 @keydown 사용 시 input 태그 활성화된 상태에서만 동작함
window.onkeydown = (e) => {
  const regExp = /[0-9]/g
  if (e.key === '1') {
    document.forms.output.value += 1;
  } else if (e.key === '2') {
    document.forms.output.value += 2;
  } else if (e.key === '3') {
    document.forms.output.value += 3;
  } else if (e.key === '4') {
    document.forms.output.value += 4;
  } else if (e.key === '5') {
    document.forms.output.value += 5;
  } else if (e.key === '6') {
    document.forms.output.value += 6;
  } else if (e.key === '7') {
    document.forms.output.value += 7;
  } else if (e.key === '8') {
    document.forms.output.value += 8;
  } else if (e.key === '9') {
    document.forms.output.value += 9;
  } else if (e.key === '0') {
    document.forms.output.value += 0;
  } else if (e.key === '.') {
    // regExp vue에서 만든 regExp 를 활용못해서 상단에 지정 필요하였음
    if (regExp.test(document.forms.output.value.slice(-1))) {
      // alert(document.forms.output.value)
      document.forms.output.value += '.';
    }
  } else if (e.key === '/') {
    if (regExp.test(document.forms.output.value.slice(-1))) {
      document.forms.output.value += '/';
    }
  } else if (e.key === '*') {
    if (regExp.test(document.forms.output.value.slice(-1))) {
      document.forms.output.value += '*';
    }
  } else if (e.key === '-') {
    if (regExp.test(document.forms.output.value.slice(-1))) {
      document.forms.output.value += '-';
    }
  } else if (e.key === '+') {
    if (regExp.test(document.forms.output.value.slice(-1))) {
      document.forms.output.value += '+';
    }
  } else if (e.key === 'Enter') {
    document.forms.output.value = eval(document.forms.output.value);
  } else if (e.key === 'Backspace') {
    document.forms.output.value = '';
  }
}

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
      resultASosujum: '0',
      resultBSosujum: '1',
      resultCSosujum: '1',
      resultDSosujum: '0',
      resultESosujum: '0',
      // 테마생성
      theme: 'default',
      themecolor: {
        "default": {
          "back": "bg-blue-300", 
          "hover": "hover:bg-blue-500", 
          "active": "bg-blue-400", 
          "text": "text-blue-800", 
          "border": "border-blue-400"
        },
        "green": {
          "back": "bg-green-300", 
          "hover": "hover:bg-green-500",
          "active": "bg-green-400",
          "text": "text-green-800", 
          "border": "border-green-400"
        },
        "pink": {
          "back": "bg-pink-300", 
          "hover": "hover:bg-pink-500", 
          "active": "bg-pink-400", 
          "text": "text-pink-800", 
          "border": "border-pink-400"
        }
      },
      cateName: '단순계산',
      // 숫자인지 확인위해 클릭과 키보드 입력시에 둘다 따로 선언하지 않고 한번에 이용하려고 하였으나 각각 필요하여서 필요 없음
      // regExp : /[0-9]/g,
    }
  },
  methods: {
    UpdateCalc(){
      // console.log("11")
    },
    bindNumber(e){
      // this.calc1 = e.target.value;
      // console.log(document.forms)
      document.forms.output.value += e;
    },
    bindOperator(e){
      // document.forms.output.value += e;
      // 오류 존재 연산자가 한개 이상 입력 되어있으면 다음 연산자는 2번 입력해야 입력이 됨 콘솔창에서 properties of null (reading 'nextSibling') 오류로 뜨는데 해결방법은 아직 모르겠음

      const regExp = /[0-9]/g; 
      if (regExp.test(document.forms.output.value.slice(-1))) {
        // alert(document.forms.output.value)
        document.forms.output.value += e;  
      }
    },
    clearOutput() {
      document.forms.output.value = '';
    },
    calcResult() {
      document.forms.output.value = eval(document.forms.output.value)
    },
    // 자리수 표현 함수
    hundredDigit(number){
      return number.replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",");
    },
    // 자리수 , 제거 함수
    deletDigit(commaString){
      return commaString.replace(/,/g, "");
    }
  },
  computed: {
    ResultCalcA() {
      // a b 계산할때는 ,만 제거 하기 위해
      let a = this.deletDigit(this.calc1);
      let b = this.deletDigit(this.calc2);
      return b === '' || a === '' ? '' : this.hundredDigit((Number(a) * Number(b/100)).toFixed(this.resultASosujum))
    },
    ResultCalcB() {
      let a = this.deletDigit(this.calc3);
      let b = this.deletDigit(this.calc4);
      return b === '' || a === '' ? '' : this.hundredDigit(((100 * Number(b)) / Number(a)).toFixed(this.resultBSosujum))
    },
    ResultCalcC() {
      let a = this.deletDigit(this.calc5);
      let b = this.deletDigit(this.calc6);
      let c = ( b - a ) / a * 100;
      return b === '' || a === '' ? '' : c > 0 ? this.hundredDigit(c.toFixed(this.resultCSosujum))+"% 증가" : this.hundredDigit(c.toFixed(this.resultCSosujum)) +"% 감소"
    },
    ResultCalcD() {
      let a = this.deletDigit(this.calc7);
      let b = this.deletDigit(this.calc8);
      return a === '' || b === '' ? '' : this.hundredDigit((Number(a) + Number(a) * Number(b / 100)).toFixed(this.resultDSosujum))
    },
    ResultCalcE() {
      let a = this.deletDigit(this.calc7);
      let b = this.deletDigit(this.calc8);
      return a === '' || b === '' ? '' : this.hundredDigit((Number(a) - Number(a) * Number(b / 100)).toFixed(this.resultESosujum))
    }

  },
  watch: {
    calc1: function() {
      return this.calc1 = this.hundredDigit(this.calc1.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc2: function () {
      return this.calc2 = this.hundredDigit(this.calc2.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc3: function () {
      return this.calc3 = this.hundredDigit(this.calc3.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc4: function () {
      return this.calc4 = this.hundredDigit(this.calc4.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc5: function () {
      return this.calc5 = this.hundredDigit(this.calc5.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc6: function () {
      return this.calc6 = this.hundredDigit(this.calc6.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc7: function () {
      return this.calc7 = this.hundredDigit(this.calc7.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
    calc8: function () {
      return this.calc8 = this.hundredDigit(this.calc8.replace(/[^0-9.]/g, '').replace(/(\..*)\./g, "$1"))
    },
  },
  }
  
</script>

<style>

</style>
