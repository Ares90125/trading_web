<template>
  <div>
    <div>
        <router-view />
    </div>
    <div class="flex fixed w-full bottom-0 bg-[#1f2029] h-[3.5rem] z-[9999] text-[#ffeba7] border-t-[1px] border-[#333]">
        <router-link to="home"  class="grow text-center text-[.7rem] mt-[0.5rem]" active-class="active">
            <BIconHouseFill class="mx-[auto] text-[25px] mb-[3px]"/>
            <p>首页</p>
        </router-link>
        <router-link to="position" class="grow text-center text-[.7rem] mt-[0.5rem]" active-class="active">
            <BIconCalendar4Range class="mx-[auto] text-[25px] mb-[3px]"/>
            <p>持仓</p>
        </router-link>
        <router-link to="service"  class="grow text-center text-[.7rem] mt-[0.5rem]" active-class="active">
            <BIconChatSquareDots class="mx-[auto] text-[25px] mb-[3px]"/>
            <p>客服</p>
        </router-link>
        <router-link to="information" class="grow text-center text-[.7rem] mt-[0.5rem]" active-class="active">
            <BIconAlarm class="mx-[auto] text-[25px] mb-[3px]"/>
            <p>资讯</p>
        </router-link>
        <router-link to="me" class="grow text-center text-[.7rem] mt-[0.5rem]" active-class="active">
            <BIconPersonCircle class="mx-[auto] text-[25px] mb-[3px]"/>
            <p>我</p>
        </router-link>
    </div>
  </div>
</template>

<style>
.index-menu div {
  flex-grow: 1;
  text-align: center;
  font-size: .7rem;
}
</style>

<script>

import { defineComponent } from 'vue'
import {BIconAlarm, BIconHouseFill,BIconClockHistory,BIconChatSquareDots,BIconCalendar4Range,BIconPersonCircle } from 'bootstrap-icons-vue';
import Content from './components/Content/index.vue';
import {useAuthStore} from '@/pinia/modules/useAuthStore';
import { mapState,mapActions  } from 'pinia'

export default defineComponent({
  name: 'layout',
  metaInfo: {
    title: 'My New Site Title'
  },
  components: {
    BIconHouseFill,
    BIconAlarm,
    BIconClockHistory,
    BIconChatSquareDots,
    BIconCalendar4Range,
    BIconPersonCircle,
    Content
  },
  data:()=>({
  }),
  computed: {
    ...mapState(useAuthStore, ['getSystem']),
  },
  methods:{
  }
});
</script>

<style>
.active{
  color : #5d7ce2;
}
</style>