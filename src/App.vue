<template>




  <!-- transition으로 애니메이션 주기  -->
  <transition name="fade">
    <Modal @closeModal="모달창열렸니 = false;" :원룸들="원룸들" :누른거="누른거" :모달창열렸니="모달창열렸니"/>
  </transition>
  
  <!-- <div class="start" :class="{end : 모달창열렸니 }">
    <Modal @closeModal="모달창열렸니 = false;" :원룸들="원룸들" :누른거="누른거" :모달창열렸니="모달창열렸니"/>
  </div> -->
  
      <div class="menu">
        <a v-for="i in 메뉴들" :key="i">{{i}} </a>
      </div>
   
  <Discount v-if="showDiscount == true"/>
  
  <button @click="priceSort">낮은가격순정렬</button>
  <button @click="highpriceSort">높은가격순정렬</button>
  <button @click="titleSort">이름순정렬</button>
  <button @click="sortBack">되돌리기</button>
  
      <h2 class="title" :style="스타일">원룸샵</h2>
    
      <!-- 반복문 코드 -->
       <!-- <div v-for="(a,i) in products" :key="i">
      <h4>{{products[i]}}</h4>
      <p >{{price[i]}}만원</p>
       </div> -->
       
       <!-- <div v-for="(a,i) in products" :key="i">
        <img src="./assets/room0.jpg" class="room-img">
    <h4>{{products[i] }}</h4>
    <p>{{price[i]}}만원</p>
    <button @click="increase(i)">허위매물신고</button><span> 신고수: {{신고수[i]}}</span>
       </div> -->
    
  <!-- 원룸들 페이지 -->
  
  <!-- <Onerooms :원룸들="원룸들" :모달창열렸니="모달창열렸니" :누른거="누른거"/> -->
  
  <Card @openModal="모달창열렸니 = true; 누른거 = $event" :원룸="원룸들[i]" v-for="(a,i) in 원룸들" :key="i"/>
  
  <!-- 누른거를 i라고 하도 됨  -->
  <!-- <Card @openModal="모달창열렸니 = true; 누른거=i" :원룸="원룸들[i]" v-for="(a,i) in 원룸들" :key="i"/> -->
  
  <!-- <Onerooms :원룸="원룸들[1]"/>
  <Onerooms :원룸="원룸들[2]"/>
  <Onerooms :원룸="원룸들[3]"/>
  <Onerooms :원룸="원룸들[4]"/>
  <Onerooms :원룸="원룸들[5]"/> -->
  
  
  
  
       <!-- <div v-for="(a,i) in 원룸들" :key="i">
  
    <img :src="원룸들[i].image" class="room-img">
    <h4 @click="모달창열렸니=true; 누른거 = i">{{원룸들[i].title}}</h4>
    <p>{{원룸들[i].price}}원</p>
  </div> -->
  
  
  
  <!-- 아래처럼 해도 됨  반목문 a로 돌려도 됨-->
  
  <!-- <div v-for="(a,i) in 원룸들" :key="i">
  
  <img :src="a.image" class="room-img">
  <h4 @click="모달창열렸니=true">{{a.title}}</h4>
  <p>{{a.price}}원</p>
  </div> -->
  
  
  
  
    <!-- <button @click="increase(0)">허위매물신고</button><span> 신고수: {{신고수[0]}}</span> -->
    
  
  
       
  <!--   
       <div>
    <img src="./assets/room1.jpg" class="room-img">
    
    <h4>{{products[1] }}</h4>
    <p>70만원</p>
    <button @click="increase(1)">허위매물신고</button><span> 신고수: {{신고수[1]}}</span>
    
       </div>
    
       <div>
     <img src="./assets/room2.jpg" class="room-img">
    
     <h4>{{products[2] }}</h4>
    <p>65만원</p>
    <button @click="increase(2)">허위매물신고</button><span> 신고수: {{신고수[2]}}</span>
    
       </div> -->
      
      </template>
      
      
      
      
      
      <script>
  
  //2023년 7월 3일 lifecycle hooks
  // create -> mount -> 컴포넌트 생성 -> update 단계 -> unmount단계 
  // 각단계전에 hook을 걸 수 있음 mehtods랑 같은 라인에 mounted(){}.만듦 
  //beforeCreate(),Created(),beforMount(), mounted,beforeUpdate(),updated(), beforeUnmount(),unmounted 등이 있음 
  // setTimeout(function(){실행할 코드}, 2000); => 2초후에 실행됨 
  // 다음처럼 arrow function 써야함 (그래야 this.변수명 써도 에러가 안남)=>  
  // mounted(){
  //   setTimeout(()=>{
  //   this.showDiscount = false;
  //         }, 2000);
  //       },
  // 위 코드의 의미 app.vue가 mount 되자 마자 위코드 실행
  // 숙제1. 메인페이지 로드 후 부터 30% 할인 문구가 1초마다 1%씩 감소 
  // 숙제2. 모달창의 input 안에 2를 기입했을때 알림창 띄우려면? 
  
  
        
  //2023 년 7월 2일 -2 상품정렬기능과 데이터 원본 보존 [...원본자료]
  //자바스크립트 데이터 정렬 후 html에 반영 
  //자바스크립트 sort 복습 var array = [3,5,3]; array.sort(function(a,b){ return a-b})
  //<button @click="priceSort">가격순정렬</button> 으로 함수넣음 
  //methods에서 함수설정 
  // priceSort(){
  //           this.원룸들.sort(function(a,b){
  //             return a.price - b.price 
  //           })
  //         },
  // 원룸들오리지널:data, 원룸들:data 이렇게 쓴다고 data의 사본이 생기지는 않음 
  //이렇게 해야함 => 원룸들오리지널:[...data], 원룸들:data 
  // 함수에서도 이렇게 => sortBack(){
  //           this.원룸들 = [...this.원룸들오리지널];
  //         },
  //  titleSort(){
  // this.원룸들.sort(function(a,b){
  //           return a.title.localeCompare(b.title);
  //         });
  //       },
  
  //2023 년 7월 2일 animation 넣는법 
  //1.시작전 class 명 2. 애니메이션 끝난 후 class명 
  //class 명을 조건부로 넣으려면 {클래스명:조건} => class ="{end: 모달창열렸니}"
  //v-if v-else 등에 <transition></transition name="fade(작명한거)">으로 감싸면 더 쉽게 애니메이션 줄 수 있음 
  // 이후 style에가서 아래와 같이 스타일링 
  // .fade-enter-from { opacity:0;}
  // .fade-enter-active { transition: all 1s;}
  // .fade-enter-to{ opacity:1;}
  // 퇴장시에는 enter를 leave로 하면 애니매이션 적용됨 
  // 위에서 밑으로 내려오기 
  // .fade-enter-from {
  //       transform: translateY(-1000px);}
  //     .fade-enter-active{
  //       transition: all 1s;
  //     }
  //     .fade-enter-to{
  //       transform: translateY(0px);
  //     }
  
  //2023 년 7월 1일 watcher로 데이터 감시하는 법
  //watch :{month(){ 사용자가 month를 글자로 입력하면 경고문 띄워주셈  } =>month라는 데이터가 변할때마다 여기있는 코드 실행됨 
  
  //2023 년 6월 30일 사용자의 input을 받는 법 (v-model)
  //모달창에서 입력란 <input @input="month = $event.target.value"> =>사용자가 입력한 값을 month(data에서 만들어 놓음)라는 변수에 넣음 
  //data binding으로 변수 바로 표시하고 원래 값에다가 변수값 곱하기 => <p> {{month}}개월 선택함 : {{원룸[누른거].price * month}}원</p>
  //위를 줄임 => <input v-model="month">
  //입력받은 값을 숫자로 저장하고 싶으면 <input v-model.number ="month">
  //좀 더 큰 인풋 => <textarea v-model="month"></textarea> =>이 때는 month의 초기값을 문자로 입력하면 문자도 변수에 잘 넣어줌! 
  //옵션도 가능 <select v-model="month">
    //<option>95</option> 
    //<option>100</option> 
    //<option>105</option>  이렇게 고른것도 자동으로 변수에 들어감 
  //체크박스도 됨 <input type="checkbox" v-model="month">=>체크는 true 체크안하면 false로 변수에 들어감
  
  
  
  //2023년 6월28일 Custom Event (자식이 부모에게 메세지를 보내게함=> 데이터 수정해달라는 메세지임) =>자식 코드에서 $emit('작명',데이터)(이거쓰면 됨)
  //부모 <onerooms에서 ~~> =>@openModal="모달창열렸니=true" 자식이 보낸 데이터를 받을 때는 $event에 담겨있음 
  //=><Onerooms  @openModal="모달창열렸니=true; 누른거=$event" :원룸="원룸들[i]" v-for="(a,i) in 원룸들" :key="i"/>
  //=>누른거 = i 도 됨 <Onerooms  @openModal="모달창열렸니=true; 누른거=i" :원룸="원룸들[i]" v-for="(a,i) in 원룸들" :key="i"/>
  //Component에 보낸 @click="모달창열렸니=true"작동안함 props로 보내서 받은 데이터는 수정금지!!(Read-only) 모달창열렸니=false로 남겠네.
  //가져온 곳에다가 바로<onerooms @click="모달창열렸니=true"/>이런식으로 해도 이벤트 버블링 현상때문에 Image, Title, price 누르면 다 상세화면 뜸  
  
  
  //2023년 6월27일
  //props 보낼때 ex) <Discount :작명(데이터이름 [보통이렇게함]) ="데이터이름(하단의 data에서 )">
  // 이렇게도 보내짐 => <Discount 데이터이름 ="안녕하세요"/> 숫자는 데이터 바인딩 해야함(콜론) <Discount :데이터이름 ="123456"/>
  //오브젝트도 보냄 <Dicount v-bind ="오브젝트"> =  <Dicount :이름 ="오브젝트.name" :나이="오브젝트.age">
  //<img :src="a.image" class="room-img"> => 오류남 a가 뭔지 모르니까!!! 
  //<img :src="원룸.image" class="room-img"> => 내가 보낸 원룸들의 이름이 [원룸]이니까 이게 맞음 아래처럼 보냄! 
  //<Onerooms :원룸="원룸들[i]" v-for="(a,i) in 원룸들" :key="i"/>
  
  
  //2023년 6월25일 -3
  // component 가져다 쓰는데 데이터까지 쓰기 
  // 데이터는 한곳에 보관하고 필요하면 가져다 쓰기 props 문법! 
  //자식(Modal) 컴포넌트가 부모가 갖고 있는 데이터 쓰려면 
  //1. 데이터 보내고/2.등록하고 / 3.쓰셈
  //<Modal:(작명 데이터이름) ="데이터이름"/> 등록할 modal.vue에 가서 props: {원룸들: Array(자료형식),}
  
  //2023년 6월25일 -2
  //component 문법 긴 HTML을 한 단어로 줄일 수 있는 문법
  //HTML이 너무 길다 한 단어로 이쁘게 축약하는 문법이 바로 component
  //component할 vue 파일에서 name을 두 단어 이상 해야함 export default {name:'Discount-page',}
  //축약해둔 컴포넌트 쓰는 법 1.vue 파일 import해오고  2.등록하고 3.쓰셈
  // import하고 component에 등록해야함 
  // components: {Discount(작명): Discount (가져온거) 걍 이렇게도 가능 => Discount},
  // 너무 많이 만들지 않는게 좋음 데이타 수정시 오류 =>ex)모달창 에서 온 데이터는 main의 데이터를 쓸 수 없음 그래서 에러 남  
  
  
  //2023년 6월25일 -1 
  //<h4> {{원룸들[사용자가 누른 상품번호].title}} </h4>을 짜야함 => data에서 변수로 만들어야함 기존거 그대로 활용X 
  //=> 누른거:0이라는 변수 위에서 원룸들[누른거] 하고 모달창 열리는 @click달았던 타이틀에가서 
  //; 이후에  추가 기능 설정 =>; 누른거= i(반복문에서 자동으로 title을 클릭할 때 마다 도는 변수)
  //동적인 UI 만드는법(자동으로 바뀌는 UI) 1. UI상태에서 저장해둘 데이터 만들기 2. 마무리는 @click등으로 UI 조작하는 버튼 만들기 
  // v-if 뒤에 v-else 쓰면 위의 if문이 참이 아니면 바로 v-else 실행 else if 문도 가능 <div v-else-if=" 1==3"> 
  // <div v-if="1==2"> 안녕하세요 </div> <div v-else> 안녕못해</div>
  
  //2023년 6월24일 
  //data 가져올 파일 앞에 바로 export default or 변수를 붙일 수 있음export default apple => apple이라는 변수를 내보낼 수 있음(페이지 하단)
  //data 열 파일에서 import apple(가져올 변수명 or 작명해도됨 어짜피 import한것만 가져옴) from './assets/oneroom.js'경로 
  // ex) import data(작명함) from'./oneroom.js';
  //HTML 태그안의 속성 데이터바인딩은 속성 앞에 :src 이렇게 써야함 
  //HTML 태그안의 내용 데이터바인딩은 이렇게 써야함 {{어쩌구}}
  
  
  
  
    //2023년 6월22일 
    //Data 어떻게 만들지 먼저 생각 어레이등등.. 
    // img 경로 점 붙여야함 => "./assetst/그림 이름 "
    // 모달창 띄우기 1.UI의 현재 상태를 데이터로 저장해둠 2.데이터에 따라 UI가 어떻게 보일지 작성 
    // v-if 원하는 html tag에 가서 v-if="1==1"<= 참일때만 실행됨 
    // vue에서 Data가 react에서 state라고 부름 vue에서도 data가 상태를 저장하는 공간으로 쓰일 수 있음 
    
    //2023년 6월21일 
    //<div v-for="(a,i) in 3" :key="i"> => a는 각각의 데이타 i는 0,1,2로 데이타 만큼 증가
    //<h4>{{products[i]}}</h4> => in products 기입시 이렇게도 가능 <h4>{{a}}</h4>
    // v-on click => @click = 변수(data에서)가 0일때 변수++ 이렇게만 해도 데이터 증가 
    // @click = "변수++" => @click = "변수 += 1" (1씩 증가 이렇게 ok)
    // @click = "함수명(script에서 함수만들어서 바로 붙임)"
    //@mouseover @mousedown 다 가능 할 듯 @치면 다 볼 수 있음 
    // 숙제 모든 원룸에 신고개수 추가 힌튼 신고수:[0,0,0]
    
    
    
    
    
    
    
    //2023년 6월20일 
    
      //비슷한 a태그 반복하기 v-for 반복문 작성법
      // 예시 <a v-for="작명 in 3(반복회수)" :key="작명">{{작명}} </a>
      //데이터 자료가져오기=> <a v-for="작명 in 메뉴들" :key="작명">{{작명}} </a>
      //key(반드시 써야함) 반복문 돌린 요소를 컴퓨터가 구분하기 위해서 씀 
      //Vue반복문 특징 1. 변수 작명 2개까지 가능 
      //2. 왼쪽 변수는 array 내의 데이터, 3오른쪽 변수는 1씩 증가하는 정수
      
     //2023년 6월20일 
    
      //데이터 바인딩 이 문법 언제 쓰는지 배워야함 => 1.변경이 쉬움 2.실시간 자동 렌더링 웹앱이 가능해짐 
      //데이타 바인딩 =>쌍중괄호 {{ }}, style은 가져올때 앞에 콜론 => :style="스타일"
      
  
      import data from'./oneroom.js';
      import Discount from './Discount.vue';
      import Modal from './Modal.vue';
      import Card from './Card.vue';
  
      export default {
        name: 'App',
        data(){
          return{
            showDiscount: true, 
            원룸들오리지널:[...data],
            오브젝트: { name:'kim', age:20},
            누른거: 0,
            원룸들:data,
            모달창열렸니: false, 
           신고수: [0,0,0],
           메뉴들 : ['Home', 'Shop', 'About', 'Contact'],
           스타일:'color:red',
           products:['역삼동원룸','천호동원룸','마포구원룸'],
           price:['50','70','65'],
          }
        },
        //함수 만드는 공간, data에서 만든 변수 쓸 때 반드시. this.신고수 이런식으로 this 붙여야함
        methods: {
          increase(i){
            this.신고수[i] +=1;
          },
          priceSort(){
            this.원룸들.sort(function(a,b){
              return a.price - b.price 
            })
          },
          highpriceSort(){
            this.원룸들.sort(function(a,b){
              return b.price - a.price 
            })
          },
          sortBack(){
            this.원룸들 = [...this.원룸들오리지널];
          },
          titleSort(){
            this.원룸들.sort(function(a,b){
              return a.title.localeCompare(b.title);
            });
          },
        },
        mounted(){
          setTimeout(()=>{
            this.showDiscount = false;
          }, 2000);
        },
        components: {
          //Discount(작명): Discount (가져온거) 걍 아래처럼 가능
              Discount, 
              Modal : Modal, 
              Card : Card,
        }
      }
      
      
      </script>
      
     
      
      <style>
       .fade-leave-from {
        opacity:1;}
      .fade-leave-active{
        transition: all 1s;
      }
      .fade-leave-to{
        opacity:0;
      }
      .fade-enter-from {
        opacity:0;}
      .fade-enter-active{
        transition: all 1s;
      }
      .fade-enter-to{
        opacity:1;
      }
  /* .fade-enter-from {
         transform: translateY(-1000px);}
      .fade-enter-active{
        transition: all 1s;
       }
       .fade-enter-to{
         transform: translateY(0px);
       } */
  
  
      .start {
        opacity: 0;
        transition: all 1s;
      }
  
      .end{
        opacity: 1;
      }
  
      body {
        margin: 0;
      }
    
      div {
        box-sizing: border-box;
      }
  
      .discount {
  background: #eee;
  padding:10px;
  margin:10px;
  border-radius:5px ;
      }
      .black-bg {
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.5);
    position:fixed; 
    padding: 20px; 
      }
    
      .white-bg {
    width: 100%; background: white;
    border-radius: 8px;
    padding: 20px; 
      }
      .room-img{
      width: 50%;
      margin-top: 40px;
    
      }
    
      #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
      }
      .menu{
      background: darkslateblue;
      padding: 15px;
      border-radius: 5px;
      }
      .menu a {
      color:white;
      padding: 10px;
      }
     
    
      </style>