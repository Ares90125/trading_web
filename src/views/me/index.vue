<template>
  <div class="bg-[#1f2029] absolute top-0 left-0 right-0 flex flex-col min-h-screen">
    <div class="bg-[#1f2029] relative">
      <!-- <div class="absolute top-0 w-full text-right p-[0.5rem]">
        <p class="inline-block text-[0.75rem] color-[#ddd]">Dead: <span class="dead">18639</span></p>
        <p class="inline-block text-[0.75rem] color-[#ddd] ml-[0.5rem]">Alive: <span class="alive">297</span></p>
        <p class="inline-block text-[0.75rem] color-[#ddd] ml-[0.5rem]">Drawn: <span class="drawn">296</span></p>
        <p class="inline-block text-[0.75rem] color-[#ddd] ml-[0.5rem]"><span class="fps">1111</span> FPS</p>
      </div> -->
      <div class="w-full bg-[#000000] h-[12rem] opacity-[.4]">
        <!-- <canvas width="1920" height="192"></canvas> -->
      </div>
      <!-- profile -->
      <div class="absolute top-[3rem] w-full flex">
        <div src="/img/logo.jpg"
          class="ml-[2rem] bg-[#ffffff] h-[4.2rem] w-[4.2rem] text-center mt-[4.2] rounded-[10px] bg-logo bg-center bg-cover bg-no-repeat	">
        </div>
        <div v-if="getUser" class="flex-1 pl-[2rem] flex-col">
          <p>用户名：<span id="UserAccount">{{getUser['name'] }}</span></p>
          <p>UID：<span id="UserUid">{{'HKD'+(52125+getUser['id']) }}</span></p>
          <p> <span class="user-money">余额： {{Number(getUser['cash_amount']).toFixed(2) }}</span> </p>
          <div class="flex flex-row text-[#ffeba7]">
            <img src="/img/cz.svg">
            <p class="ml-[5px]" @click="()=>{$router.push({ name: 'recharge' })}">充值 |</p>
            <img class="ml-[5px]" src="/img/tx.svg">
            <p class="ml-[5px]" @click="()=>{$router.push({ name: 'withdrawal' })}"> 提现</p>
          </div>
        </div>
      </div>
    </div>
    <!-- list -->
    <div class="overflow-y-auto overflow-x-hidden	h-full bg-[#2b2f3e] w-full">
      <ul class="rounded-0 cursor-pointer ">
        <li @click="recharge" class="border-b-[1px] bg-[#1f2029] border-gray-700	 text-[#ddd] px-[1.25rem] py-[0.75rem]">
          <BIconLifePreserver  class="text-[#ffeba7] inline-block text-[1rem] mr-[0.3rem]"/>
          充值记录
        </li>
        <li @click="withdrawal" class="border-b-[1px] bg-[#1f2029] border-gray-700 text-[#ddd] px-[1.25rem] py-[0.75rem]">
          <BIconCreditCard2Back class="text-[#ffeba7] inline-block text-[1rem]  mr-[0.3rem]"/>
          提现记录
        </li>
        <li @click="news" class="border-b-[1px] bg-[#1f2029] border-gray-700 text-[#ddd] px-[1.25rem] py-[0.75rem]">
          <BIconNewspaper class="text-[#ffeba7] inline-block text-[1rem]  mr-[0.3rem]"/>
          新闻公告</li>
        <li @click="changepassword" class="border-b-[1px] bg-[#1f2029] border-gray-700 text-[#ddd] px-[1.25rem] py-[0.75rem]">
          <BIconPencilSquare class="text-[#ffeba7] inline-block text-[1rem]  mr-[0.3rem]"/>
          修改密码</li>
        <li  @click="signOut"  class="border-b-[1px] bg-[#1f2029] border-gray-700 text-[#ddd] px-[1.25rem] py-[0.75rem]">
          <BIconPower class="text-[#ffeba7] inline-block text-[1rem]  mr-[0.3rem]"/>
          退出登陆</li>
      </ul>
    </div>
  </div>
</template>

<script>
layer.config({
  skin: 'me-class'
})
import { defineComponent } from 'vue'
import {
  BIconHouseFill,
  BIconClockHistory,
  BIconChatSquareDots,
  BIconCalendar4Range,
  BIconPersonCircle,
  BIconLifePreserver,
  BIconCreditCard2Back,
  BIconNewspaper,
  BIconPencilSquare,
  BIconPower,
} from 'bootstrap-icons-vue';
import {useAuthStore} from '@/pinia/modules/useAuthStore';
import { mapState,mapActions  } from 'pinia'

export default defineComponent({
  name: 'me',
  components: {
    BIconHouseFill,
    BIconClockHistory,
    BIconChatSquareDots,
    BIconCalendar4Range,
    BIconPersonCircle,
    BIconLifePreserver,
    BIconCreditCard2Back,
    BIconNewspaper,
    BIconPencilSquare,
    BIconPower,
  },
  data: () => ({
    checked: true,
    visible:true

  }),
  computed:{
    ...mapState(useAuthStore, ['getUser']),
  },
  methods: {
    ...mapActions(useAuthStore, ['logout']),
    recharge(){
      this.$router.push({ name: 'rechargehistory' })
    },
    withdrawal(){
      this.$router.push({ name: 'withdrawalhistory' })
    },
    news(){
      this.$router.push({ name: 'news' })
    },
    changepassword(){
      this.$router.push({ name: 'changepassword' })
    },
    signOut(){
      layer.config({
        skin: 'me-class'
      })
      layer.open({
        title:false,
        content: '立即退出？',
        btn:['取消','确定'],
        btnAlign: 'c',
        closeBtn: 0,
        shadeClose:1,
        btn2 :()=>{
          this.logout();
          this.$router.push({ name: 'login' })
        }
      });
    },
  }
})
</script>
<style>

</style>