<template>
  <div>
    <!-- <button id="btnAdd" class="m-btn m-btn-default" v-on:click="btnAddOnClick">
      <div class="m-btn-icon icon-add"></div>
      <div class="btn-text">Thêm nhân viên</div>
    </button> -->
    <div
      class="m-dialog dialog-detail"
      title="Thêm mới cửa hàng"
      :class="{ isHide: isHide }"
    >
      <div class="dialog-modal"></div>
      <div class="dialog-content">
        <div class="dialog-header">
          <div class="dialog-header-title">Thêm mới cửa hàng</div>
          <div class="dialog-header-close">
            <button v-on:click="btnCancelOnClick">x</button>
          </div>
        </div>
        <div class="dialog-body">
          <div class="m-row m-flex">    
            <div class="m-col el-left m-flex-4">
              <div class="m-row mg-top-0 m-flex">
                <div class="m-col m-flex-1" style="display:flex;">
                  <div class="m-label">
                    Mã cửa hàng (<span class="label-required">*</span>)
                  </div>
                  <div class="m-control" style="flex-grow :1;">
                    <input
                      id="txtShopCode"
                      fieldName="ShopCode"
                      required
                      class="input-required"
                      type="text"
                      ref ="shopCode"
                      v-model="shopDetail.shopCode" 
                    >
                    <span id='shopCode-warning-icon' class="warning-icon isHide">&nbsp;</span>
                    <span id='shopCode-warning-text' class="label-required isHide"> Không được để trống Mã cửa hàng</span>
                  </div>
                </div>
              </div>
              <div class="m-row m-flex">
              <div class="m-col m-flex-1" style="display:flex;">
                  <div class="m-label">
                    Tên cửa hàng (<span class="label-required">*</span>)
                  </div>
                  <div class="m-control" style="flex-grow :1;">
                    <input
                      id="txtShopName"
                      fieldName="ShopName"
                      required
                      class="input-required"
                      type="text"
                      v-model="shopDetail.shopName"    
                    />
                    <span id='shopName-warning-icon' class="warning-icon isHide">&nbsp;</span>
                    <span id='shopName-warning-text' class="label-required isHide">Không được để trống Tên cửa hàng</span>
                  </div>
                </div>
                </div>
                <div class="m-row m-flex">
                <div class="m-col m-flex-1" style="display:flex;">
                  <div class="m-label">
                    Địa chỉ (<span class="label-required">*</span>)
                  </div>
                  <div class="m-control" style="flex-grow :1;">
                    <textarea
                      id="txtShopAddress"
                      fieldName="ShopAddress"
                      required
                      class="input-required"
                      type="text"
                      v-model="shopDetail.shopAddress"    
                    />
                    <span id='shopAddress-warning-icon' class="warning-icon isHide">&nbsp;</span>
                    <span id='shopAddress-warning-text' class="label-required isHide">Không Được để trống Địa chỉ Cửa hàng</span>
                  </div>
                </div>
              </div>
              <div class="m-row m-flex">
                <div class="m-flex-1" style="display:flex;">
                  <div class="m-label">Số điện thoại</div>
                  <input
                    id='txtShopPhoneNumber'
                    class="m-combobox-input"
                    type="text"
                    autocomplete="on"
                    v-model="shopDetail.shopPhoneNumber"
                  />
                </div>
                <div class="m-flex-1 mg-left-10px" style="display:flex;">
                  <div class="m-label">Mã số thuế</div>
                  <input
                    id='txtShopTaxCode'
                    class="m-combobox-input"
                    type="text"
                    autocomplete="on"
                    v-model="shopDetail.shopTaxCode"
                  />
                </div>
              </div>
              <div class="m-row m-flex">
                <div class="m-flex-1" style="display:flex;">
                  <div class="m-label">Quốc Gia</div>
                    <select
                    id="cbxNation"
                    fieldName="Nation"
                    api="/api/customergroups"
                    class="m-control"
                    v-model="shopDetail.shopNation"
                    >
                     <option value="Việt Nam">Việt Nam</option>
                    </select>
                   
                </div>
                <div class="m-flex-1 mg-left-10px"></div>
              </div>
              <div class="m-row m-flex">
                <div class="m-flex-1" style="display:flex;">
                  <div class="m-label">Tỉnh/ Thành phố</div>
                  <select
                    id="cbxCity"
                    fieldName="City"
                    fieldValue="CityId"
                    api="/api/customergroups"
                    class="m-control"
                    :required="true"
                    @change="setSelectedCity"
                    v-model="shopDetail.cityName"
                    
                  >
                  <option value="" disabled>Nhấp để tìm kiếm thành phố</option>
                  <option  v-for="city in cities" :key="city.cityID"
                    :value="city.cityName">
                   {{city.cityName}}
                   </option>
                  </select>
                </div>
                <div class="m-flex-1 mg-left-10px" style="display:flex;">
                  <div class="m-label">Quận / Huyện</div>
                  <select
                    id="cbxDistrict"
                    fieldName="ShopDistrict"
                    fieldValue="DistrictId"
                    api="/api/customergroups"
                    class="m-control"
                    v-model="shopDetail.districtName"
                    @change="setSelectedDistrict"
                  >
                  <option value="" disabled >Nhấp để tìm kiếm quận/ huyện </option>
                  <option v-for="district in districts" :key="district.districtID" :value="district.districtName">{{district.districtName}}</option>
                  </select>
                </div>
              </div>
              <div class="m-row m-flex">
                <div class="m-flex-1" style="display:flex;">
                  <div class="m-label">Phường/ Xã</div>
                  <select
                    id="cbxWard"
                    fieldName="ShopWardName"
                    fieldValue="ShopWardId"
                    api="/api/customergroups"
                    class="m-control"
                    v-model="shopDetail.wardName"
                    @change="setSelectedWard"
                  >
                  <option value="" disabled>Nhấp để tìm kiếm phường / xã</option>
                  <option v-for="ward in wards" :key="ward.wardID" :value="ward.wardName">{{ward.wardName}}</option>
                  </select>
                   
                </div>
                <div class="m-flex-1 mg-left-10px" style="display:flex;">
                  <div class="m-label">Đường phố</div>
                  <input
                    id='txtShopStreetName'
                    class="m-combobox-input"
                    type="text"
                    autocomplete="on"
                    v-model="shopDetail.shopStreet"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="dialog-footer">
          <button id="btnEdit"  class="m-btn m-btn-default" @click="editShop">
            <span id='edit-button'>&nbsp;</span>
            <span class="btn-text">Lưu</span>
          </button>
          <button id="btnSave" class="m-btn m-btn-default" @click="saveShop">
            <span id='add-button'>&nbsp;</span>
            <span class="btn-text">Lưu và thêm mới</span>
          </button>
          <button id="btnCancel" class="m-btn m-btn-default m-btn-cancel" v-on:click="btnCancelOnClick">
            Hủy
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  name:"ShopDetail",

  props:{
    isHide : Boolean,
    shopDetail : Object
  },

  data() {
    return {
      dialog: false,
      display: "none",
      cities : [],
      districts : [],
      wards : []
    };
  },

  methods: {
    
    // 1.Nhóm dữ liệu thực hiện chức năng các button

    // Lưu thông tin shop
    async saveShop(){
      if (this.validate() == true ) 
      {
        console.log(this.shopDetail);
        axios.post("https://localhost:44348/api/v1/Shop",this.shopDetail)
        .then(() => this.$emit('CloseAndUpdate','add'))
        .catch( error => this.$emit('bad-request',error.response.data.message) )
      }
      else {
        // Hiển thị Icon cảnh báo
        document.getElementById("shopCode-warning-icon").classList.remove("isHide")
        document.getElementById("shopName-warning-icon").classList.remove('isHide')
        document.getElementById("shopAddress-warning-icon").classList.remove('isHide')
       
        // Hiển thị thông báo cảnh báo
        document.getElementById("shopCode-warning-text").classList.remove("isHide")
        document.getElementById("shopName-warning-text").classList.remove('isHide')
        document.getElementById("shopAddress-warning-text").classList.remove('isHide')
      }
    },
    // Sửa thông tin shop
    async editShop(){
      if (this.validate() == true ) 
      {
        axios.put("https://localhost:44348/api/v1/Shop",this.shopDetail)
        .then(() => this.$emit('CloseAndUpdate','edit'))
        .catch( error => this.$emit('bad-request',error.response.data.message) )
      }
      else 
      {
        // Hiển thị Icon cảnh báo
        document.getElementById("shopCode-warning-icon").classList.remove("isHide")
        document.getElementById("shopName-warning-icon").classList.remove('isHide')
        document.getElementById("shopAddress-warning-icon").classList.remove('isHide')
       
        // Hiển thị thông báo cảnh báo
        document.getElementById("shopCode-warning-text").classList.remove("isHide")
        document.getElementById("shopName-warning-text").classList.remove('isHide')
        document.getElementById("shopAddress-warning-text").classList.remove('isHide')
      }
    },

    // Đóng Form 
    btnCancelOnClick() {
      this.$emit('Close')
    },

    // Nhóm Method để lấy dữ liệu : quận, huyện

    // Lấy dữ liệu quận
    async getDistrict(){
      var url = "https://localhost:44348/GetDistrictByCity?CityID=";
      url +=this.shopDetail.cityID;
      if(this.shopDetail.cityID != null){
      const response = await axios.get(url);
      this.districts = response.data.data;
      }
    },

    // Lấy dữ liệu Phường / Xã
    async getWard(){
      var url ="https://localhost:44348/GetWardByDistrict?DistrictID=";
      url += this.shopDetail.districtID;
      if (this.shopDetail.districtID != null){
      const response = await axios.get(url);
      this.wards = response.data.data;
      }
    },


    // 3.Nhóm methods gắn các trường ( cityID,wardID,DistrictID)

    // Gắn trường cityID cho Đối tượng shop
    async setSelectedCity(){
        var url = "https://localhost:44348/GetCityIDByName?CityName=";
        url += this.shopDetail.cityName;
        const response = await axios.get(url);
        this.shopDetail.cityID = response.data.data;
        this.getDistrict()
    },
    // Gắn trường districtID cho Đối tượng quận
    async setSelectedDistrict(){
      var url= "https://localhost:44348/GetDistrictID?CityID="
      url += this.shopDetail.cityID+"&DistrictName="+this.shopDetail.districtName;
      const response = await axios.get(url);
      this.shopDetail.districtID = response.data.data;
      this.getWard()
    },

    // Gắn trường wardID cho đối tượng Phường / Xã
    async setSelectedWard(){
      var url= "https://localhost:44348/GetWardID?DistrictID="
      url += this.shopDetail.districtID+"&WardName="+this.shopDetail.wardName;
      const response = await axios.get(url);
      this.shopDetail.wardID = response.data.data;
    },


    // 4.Nhóm methods thực hiện validate dữ liệu:
    validate(){
      var rightFormat = true;
      var shopCode = document.getElementById('txtShopCode');
      var shopName = document.getElementById('txtShopName');
      var shopAddress = document.getElementById('txtShopAddress');
  
      if(shopCode.value.length == 0 ){
        rightFormat = false;
        
      }
      if (shopName.value.length == 0) {
        rightFormat = false;
        
      }
      if (shopAddress.value.length == 0){
         rightFormat = false; 
      }
      return rightFormat;
    },
    focusOnInput(){
      this.$refs.shopCode.focus();
    }
  },
  async created(){
    const response = await axios.get("https://localhost:44348/api/v1/City");
    this.cities = response.data.data;
  },
  watch:{
    'shopDetail.cityID' : function(){
      this.getDistrict()
    },
    'shopDetail.districtID' : function(){
      this.getWard()
    },
    'shopDetail.shopCode' : function(){
      setTimeout(this.focusOnInput,200)
    }
  }
};
</script>
<style scoped>
.isHide {
  display: none;
}

.m-control{
  position: relative;
}

.m-dialog {
  z-index: 100;
}

.dialog-header {
  position: relative;
  height: 40px;
  line-height: 60px;
  padding-left: 16px;
  display: flex;
  font-size: 24px;
}

.dialog-header-close {
  position: absolute;
  right: 16px;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  cursor: pointer;
  top: 10px;
  align-items: center;
  border: none;
  background-color: transparent;
  font-size: 24px;
  line-height: 24px;
}
.dialog-modal {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: black;
  opacity: 0.4;
  z-index: 99;
}

.dialog-content {
  position: fixed;
  border-radius: 5px;
  width: 750px;
  background-color: #fff;
  left: calc(50% - 325px);
  top: calc(50% - 450px);
  z-index: 100;
}
.dialog-body {
  padding: 0 16px 16px 16px;
}
.dialog-footer {
  display: flex;
  width: 100%;
  height: 60px;
  background-color: #e9ebee;
  border-radius: 0 0 5px 5px;
  align-items: center;
  justify-content: flex-end;
  padding: 12px 24px;
  box-sizing: border-box;
}
.el-avatar-employee {
  padding-top: 16px;
  padding-right: 16px;
}
.el-avatar-note {
  font-size: 12px;
}

.currency-for-input {
  position: absolute;
  right: 40px;
  line-height: 40px;
  font-style: italic;
}

.warning-icon{
  position: absolute;
  right: 5px;
  top:15px;
  background: url("../../../assets/icon/common-icon.png") no-repeat -303px -3px;
  height: 20px;
  width: 20px;
}

#edit-button{
  position: absolute;
  left: 8px;
  top:15px;
  background: url("../../../assets/icon/common-icon.png") no-repeat -321px -129px;
  height: 20px;
  width: 20px;
}
#btnEdit{
  background-color: #005082;
  color: white;
}

#add-button{
  position: absolute;
  left: 5px;
  top:15px;
  background: url("../../../assets/icon/common-icon.png") no-repeat -25px -130px;
  height: 20px;
  width: 20px;
}
#btnSave{
  background-color: white;
  color:#005082 ;
  border: 1px solid #005082;
}
</style>

