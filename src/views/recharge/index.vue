<template>
  <div class="absolute top-0 left-0 bottom-0 right-0 flex flex-col">
    <!-- header -->
    <div class="h-[2.5rem] w-full text-center bg-[#1f2029] pl-[10px] py-[8px]">
      <div class="float-left absolute">
        <BIconPersonCircle @click="back" class="text-[1.3rem] cursor-pointer mt-[3px]" />
      </div>
      <div class="font-black text-white">充值</div>
    </div>
    <!-- card -->
    <div class="m-3 bg-[#DC3545] p-4 flex flex-row justify-between">
      <div class="flex">
        <div class="rounded-full w-[48px] h-[48px] text-center justify-center flex items-center bg-[#FF8792] mr-[10px]">
          <p>{{ getUser.bank.name.slice(0,1) }}</p>
        </div>
        <div class="flex flex-col justify-evenly">
          <p>{{getUser.bank.name}}</p>
          <p>{{`${getUser.bank.cardnumber.toString().slice(0,3)} **** **** **** **** ${getUser.bank.cardnumber.toString().slice(-3)}`}}</p>
        </div>
      </div>
      <button
          class="my-2" style="padding:0.75rem" @click="updateCard">编辑</button>
    </div>
    <p class="px-3">
      余额：{{ getUser.cash_amount }} 
    </p>
    <!-- body -->
    <div class="overflow-x-hidden p-3">
      <div>
        <ul class="mt-2 list-group-item list-group-flush">
          <li class="list-group-item flex items-center px-[20px] py-[15px]">
            <BIconLifePreserver class="bi-life-preserver"/>
            <input type="number" name="money" placeholder="充值金额" v-model="form.amount" class="ml-[10px] ml-[10px] input-transparent p-1 text-[16px]" id="CashMoney">
          </li> 
          <li class="list-group-item flex items-center px-[20px] py-[15px]">
            <BIconShieldExclamation class="bi-shield-exclamation" />
            <input type="password" name="money" placeholder="安全密码" v-model="form.security" class="ml-[10px] input-transparent p-1 text-[16px]" id="CashSafePawd">
          </li> 
        </ul>
        <button
          class="my-2 btn btn-success btn-block btn-sm " style="padding:0.75rem" @click="sendRequest" :disabled="loading">确定</button>
      </div>
    </div>
  </div>
</template>


<script>

import { defineComponent } from 'vue'
import { BIconPersonCircle,BIconLifePreserver,BIconShieldExclamation } from 'bootstrap-icons-vue';
import {useAuthStore} from '@/pinia/modules/useAuthStore';
import { mapState,mapActions  } from 'pinia'
import axios from 'axios'
layer.config({
  skin: 'error-class'
})
export default defineComponent({
  name: 'recharge',
  components: {
    BIconPersonCircle,
    BIconLifePreserver,
    BIconShieldExclamation 
  },
  data: () => ({
    loading:false,
    message:'',
    form:{
      bank_id:null,
      amount:null,
      security:null,
    }
  }),
  computed:{
    ...mapState(useAuthStore, ['getUser','getReturnUrl']),
  },
  methods: {
    updateCard(){
      this.$router.push({ name: 'bankedit' });
    },
    getCardNum(cardnum){
      var len = cardnum.length -3;
      return cardnum.slice(len);
    },
    back() {
      this.$router.push({ name: 'me' });
    },
    showDialog(){
      layer.open({
          type:1,
          offset:'b',
          title:false,
          content: this.message,
          closeBtn: 0,
          shadeClose:1,
      });
    },
    async sendRequest(){
      this.loading=true;
      this.form.bank_id=this.getUser.bank.id;
      if(this.validation()){
            try{
                const response=await axios.post('/recharge', this.form);
                if(response.data.status==1){
                  this.form.amount=null;
                  this.form.security=null;
                    layer.open({
                        type:1,
                        offset:'b',
                        title:false,
                        content: '成功',
                        closeBtn: 0,
                        shadeClose:1,
                    });
                }else{
                   this.message=response.data.message;;
                    this.showDialog();
                }
            }
            catch(error) {
                this.showDialog();
            };
        }
        else{
            this.showDialog();
        }
        this.loading=false;
    },
    validation(){
        if(this.form.amount==null||this.form.security==null){
            this.message='请输入所有值';
            return false;
        }
        if(this.form.security<6){
            this.message='安全密码必须为6位或更长';
            return false;
        }
        return true;
    },
  }
})
</script>
<style>
</style>