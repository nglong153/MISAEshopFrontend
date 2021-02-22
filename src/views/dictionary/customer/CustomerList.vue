<template>
  <div>Đây là trang khách hàng
    <SelectBox :default-option ='defaultOption' :options = 'options' @selected='handleSelectBox' />
    <popup :popup-icon="'joijoi'" :popup-content="'Bạn có chắc muốn xóa thông tin về nhân viên này không?'" :popup-type="'danger'" :popup-header="'Xóa thông tin nhân viên '"/>
    <popup :popup-icon="'joijoi'" :popup-content="'Bạn có chắc ?'" :popup-type="'noti'" :popup-header="'Popup Thông Báo gì đó'"/>
  </div>
</template>
<script>
import SelectBox from '../../../components/SelectBox.vue';
import Popup from '../../../components/Popup.vue'

export default {
  name: "Customers",
  components:{
    SelectBox,
    Popup
  },
  data() {
    return {
      selectedCustomer: {
        CustomerId: 1,
        FullName: "Nguyễn Văn Mạnh",
      },
      customers: [{ CustomerId: 1, FullName: "Nguyễn Văn Mạnh" }],
      defaultOption : 'Tất cả các phòng ban',

      options : 
      [
        {
          name :'option 1',
          id : 1
        },
        {
          name:'option 2',
          id : 2
        }, 
        {
          name:'option 3',
          id: 3
        }
      ]
    };
  },
  methods : {
    handleSelectBox(optionId){
      this.options.forEach(element => {
        if (element.id == optionId)
        this.defaultOption = element.name
      })
    }
  }
};
</script>
<style scoped>


</style>