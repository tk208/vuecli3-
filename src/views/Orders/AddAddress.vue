<template>
    <div class="addAddress">
        <Header :isLeft="true" :title="title" />
        <div class="viewbody">
            <div class="content">
                <FormBlock v-model="addressInfo.name" :selectSex="addressInfo.sex" @checkSex="checkSex" :tags="sexes" label="联系人" placeholder="姓名"  />
                <FormBlock v-model="addressInfo.phone" label="电话" placeholder="手机号码"  />
                <FormBlock v-model="addressInfo.address" @click="showSearch = true" label="地址" 
                icon="angle-right"
                placeholder="小区/写字楼/学校等"  />
                <FormBlock label="门牌号" 
                v-model="addressInfo.bottom"
                icon="edit"
                textarea="textarea"
                placeholder="10号楼5单元404"  />
                <div class="formblock">
                  <div class="label-wrap">
                    标签
                  </div>
                   <TabTag :selectTag="addressInfo.tag" :tags="tags" @checkTag="checkTag" />
                </div>
            </div>
            <div class="form-button-wrap">
              <button @click="handleSave" class="form-button">确定</button>
            </div>
        </div>
        <AddressSearch :addressInfo="addressInfo" @close="showSearch = false" :showSearch="showSearch" />
    </div>
</template>

<script>
import Header from '../../components/Header'
import FormBlock from '../../components/Orders/FormBlock'
import TabTag from '../../components/Orders/TabTag'
import AddressSearch from '../../components/Orders/AddressSearch'
import { Toast } from 'mint-ui';
export default {
    name: 'AddAddress',
    data(){
        return {
            title: '添加地址',
            tags: ['家','学校','公司'],
            sexes: ['先生','女士'],
            addressInfo: {},
            showSearch: false
        }
    },
    components: {
        Header,
        FormBlock,
        TabTag,
        AddressSearch
    },
    beforeRouteEnter(to,from,next){
      next(vm => {
        vm.addressInfo = to.params.addressInfo
        vm.title = to.params.title
      })
    },
    methods: {
      checkTag(item){
        this.addressInfo.tag = item
      },
      checkSex(item){
        this.addressInfo.sex = item
      },
      handleSave(){
        // console.log(this.addressInfo)
        if(!this.addressInfo.name){
          this.showMsg('请输入联系人')
          return
        } else if(!this.addressInfo.phone){
          this.showMsg('请输入手机号')
          return
        } else if(!this.addressInfo.address){
          this.showMsg('请输入收货地址')
          return
        }
        // 存储数据
        if(this.title == '添加地址'){
          this.addAddressInfo()
        } else {
          this.editAddress()
        }

      },
      showMsg(msg){
        Toast({
          message: msg,
          position: 'bottom',
          duration: 2000
        });
      },
      addAddressInfo(){
        this.$axios.post(`/api/user/add_address/${localStorage.ele_login}`,this.addressInfo)
          .then(res => {
            if(!this.$store.getters.userInfo){
              this.$store.dispatch('setUserInfo',this.addressInfo)
            }
            this.$router.push('/myAddress')
          })
          .catch(err => {
            console.log(err)
          })
      }
    },
    editAddress(){
      this.$axios.post(`/api/user/edit_address/${localStorage.ele_login}/${this.addressInfo._id}`,this.addressInfo)
        .then(res => {
          this.$router.push('/myAddress')
        })
    }
}
</script>

<style scoped>
.addAddress {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding-top: 45px;
}
.viewbody {
  width: 100%;
  height: 100%;
  overflow: auto;
  box-sizing: border-box;
  background-color: #f5f5f5;
}

.content {
  padding-left: 4vw;
  background: #fff;
  font-size: 0.94rem;
}

.formblock {
  background-color: #fff;
  border-bottom: 1px solid #eee;
  display: flex;
}
.formblock .label-wrap {
  flex-basis: 17.333333vw;
  padding: 3.733333vw 0;
  line-height: 4.8vw;
  color: #333;
  font-weight: 700;
}

/* 确定按钮 */
.form-button-wrap {
  padding: 5.333333vw 4vw;
  display: flex;
}
.form-button-wrap .form-button {
  background: #00d762;
  text-align: center;
  border-radius: 0.533333vw;
  flex: 1;
  font-size: 1.1rem;
  line-height: 5.066667vw;
  color: #fff;
  padding: 3.333333vw 0;
  border: none;
  font-weight: 500;
}
</style>