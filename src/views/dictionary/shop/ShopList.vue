<template>
  <div class="content-body">
     <toast-message :is-hide-toast="isHideToast" :toast-content="toastContent"/>
    <div class="header-content">
      <div class="title">Danh sách cửa hàng</div>
      <!-- < class="content-feature"> -->
     
      <shop-detail @bad-request='BadRequestHandler'  @CloseAndUpdate="reloadAfterEdit" 
                   @Close='CloseForm' :isHide = "isHideParent" :shopDetail="selectedShop" 
                    />
      <popup @execute="deleteShop" @popupClose="ClosePopup" 
            :popup-state="isHidePopup" 
            :popup-content="popupContent" 
            :popup-type="popupType" 
            :popup-header="popupHeader"/>
    </div>
    
    <!-- <v-data-table
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      class="elevation-1"
    >
    </v-data-table> -->
    <div class="tool-bars">
      <div class="tool-holder" @click="btnAddOnClick">
        <div class="icon add-icon"></div>
        <span>Thêm mới</span>
      </div>
      <div class="tool-holder" style="opacity:0.2; pointer-events:none;">
        <div class="icon duplicate-icon"></div>
        <span>Nhân bản</span>
      </div>
      <div class="tool-holder" @click="deleteButtonClicked">
        <div class=" icon delete-icon"></div>
        <span>Xóa</span>
      </div>
      <div class="tool-holder" @click="editButtonClicked">
        <div class="icon edit-icon"></div>
        <span>Sửa</span>
      </div>
      <div class="tool-holder" @click="GetShopList">
        <div class="icon reload-icon"></div>
        <span>Nạp</span>
      </div>
    </div>
    <div class="table-container">
    <loading :is-loading="isLoading"/>
    <table id="tbListData"
        cellspacing="0"
        cellpadding="0"
        border="0"
        class="el-table__body"
        style="min-width: 100%">
      <thead>
        <tr>
          <th > Mã Nhà Hàng</th>
          <th  colspan="2">Tên Nhà Hàng</th>
          <th  colspan="2">Địa Chỉ</th>
          <th>Số Điện Thoại</th>
          <th>Trạng thái</th>
        </tr>
      </thead>
      <tr>
          <td>
            <div class="filter-holder">
              <div class="filter-type" >
                <span>*</span>
              </div>
            <input class = "filter-input" type="text">
            </div>
          </td>
          <td colspan="2">
            <div  class="filter-holder">
              <div class="filter-type" >
                <span>*</span>
              </div>
            <input class = "filter-input" type="text">
            </div>
          </td>
          <td colspan="2">
            <div class="filter-holder">
              <div class="filter-type" >
                <span>*</span>
              </div>
            <input class = "filter-input" type="text">
            </div>
          </td>
          <td>
            <div class="filter-holder">
              <div class="filter-type" >
                <span>*</span>
              </div>
            <input class = "filter-input" type="text">
            </div>
          </td>
          <td>
           <select  class = "filter-input" name="shop-status" id="shop-status">
             <option value="0">Ngừng Hoạt Động</option>
             <option value="1">Đang Hoạt Động</option>
             <option value="2" selected>Tất cả trạng thái</option>
           </select>
          </td>
      </tr>
      <tbody>
        <tr class="data-row" v-for="shop in shops" :key="shop.shopID" :id="shop.shopID" @click=rowOnClick(shop.shopID) @dblclick="rowOnDblClick(shop.shopID)">
          <td>{{shop.shopCode}}</td>
          <td colspan="2">{{shop.shopName}}</td>
          <td colspan="2">{{shop.shopAddress}}</td>
          <td>{{shop.shopPhoneNumber}}</td>
          <td v-if ="shop.shopStatus == 1">
            <span>Đang Hoạt Động</span>
          </td>
          <td v-else  >
             <span>Ngừng hoạt động </span>
          </td>
        </tr>
      </tbody>
    </table>
    </div>
    <div class="paging-bar"> 
      <div class="paging-option">
        <div class="btn-select-page m-btn-firstpage"></div>
        <div class="btn-select-page m-btn-prev-page"></div>
        <div class="m-btn-list-page" style="display:; align-items:center; justify-content: center">
          <span style="margin-right:10px">Trang</span>
          <input type="text" style="width:45px ;height:30px;" placeholder="1" >
          <span style = "margin-left : 10px" >Trên 20</span>
        </div>
        <div class="btn-select-page m-btn-next-page"></div>
        <div class="btn-select-page m-btn-lastpage"></div>
      </div>
      <select name="record-per-page" id="record-per-page">
        <option value="10" >10</option>
        <option value="20">20</option>
        <option value="50" selected>50</option>
      </select>
      <div class="paging-record-info">Hiển thị <b>1-50/200</b> kết quả</div>
    </div>
  </div>
</template>
<script>
import * as axios from "axios";
import ShopDetail from './ShopDetail.vue';
import Popup from '../../../components/Popup.vue';
import Loading from '../../../components/Loading.vue';
import ToastMessage from '../../../components/ToastMessage.vue'

export default {
  name: "Shop",
  components: {
    ShopDetail,
    Popup,
    Loading,
    ToastMessage
  },
  methods: {
    // 1. Nhóm methods xử lý event của các nút chức năng
    btnAddOnClick() {
      this.isHideParent = !this.isHideParent;
      this.selectedShop = {}
      this.selectedShop.cityName = "",
      this.selectedShop.districtName = "",
      this.selectedShop.wardName = ""
    },
    
    editButtonClicked(){
      this.isHideParent = !this.isHideParent;
    },
    deleteButtonClicked()
    {
      
      if(this.selectedShop.shopID != null )
      {
        if (this.popupContent != "Bạn có chắc muốn xóa ")
        {
          this.popupContent = "Bạn có chắc muốn xóa ";
          this.popupHeader = "Xóa thông tin nhân viên ";
          this.popupType ='danger';
        }
        this.popupContent += this.selectedShop.shopCode +' khỏi danh sách cửa hàng không?'
      }
      else{ 
        this.popupHeader="Thông báo"
        this.popupContent = "Chọn 1 bản ghi để tiến hành xóa"
      }
      this.isHidePopup = false;
    },
    preventUncheck()
    {
      var checkboxList = document.querySelectorAll(".status-checkbox")
      checkboxList.forEach( checkbox => {
        checkbox.addEventListener("click", function(e){
          if(checkbox.checked )
          {
          e.preventDefault();
          }
        })
      })
    },
    // 2. Nhóm Methods xử lý thao tác của người dùng trên bảng dữ liệu
    async rowOnClick(rowID){
      var selectedRow = document.getElementById(rowID);
      var rows = document.querySelectorAll(".data-row");
      for (let row of rows){
        if (row.classList.contains('selected-row')){
          row.classList.remove('selected-row');
          break;
        }
      }
      selectedRow.classList.add('selected-row');
      for (let shop of this.shops){
        if(shop.shopID == rowID){
          this.selectedShop = shop;
          break;
        }
      }
    },
    async rowOnDblClick(rowID){
      this.rowOnClick(rowID).then(this.editButtonClicked)
    },
    deleteShop()
    {
      for (let [i,shop] of this.shops.entries())
        {
          if (shop.shopID == this.selectedShop.shopID)
          {
            this.isHidePopup = false;
            axios.delete("https://localhost:44348/api/v1/Shop", 
            {
            data: [this.selectedShop.shopID]
            }).then( this.shops.splice(i,1))
            .then(this.isHidePopup = true)
          }
         }
    }
    ,
    // 3. Nhóm Methods xử lý sự kiện được con emit lên
    CloseForm(){
      this.isHideParent = true;
      var rowID = "";
      var rows = document.querySelectorAll(".data-row");
      for (let row of rows){
        if (row.classList.contains('selected-row')){
          for (let shop of this.shops)
          {
            if(shop.shopID == row.id)
            {
              this.selectedShop = shop;
              console.log(row.id)
              break;
             }
          }
          break;
        }
      }

      for (let shop of this.shops){
        if(shop.shopID == rowID){
          this.selectedShop = shop;
          break;
        }
      }
    },
    ClosePopup(){
      this.isHidePopup = true;
    },
    BadRequestHandler(badRequest){
      this.popupContent ="";
      for (let message of badRequest){
        this.popupContent += message;
      }
      this.popupHeader = "Thông báo"
      this.popupType = "";
      this.isHidePopup = false;
    },
    reloadAfterEdit(type){
      if(type == 'add') {
        this.toastContent = 'Thêm mới cửa hàng thành công'
      }
      if (type == 'edit'){
        this.toastContent = "Sửa cửa hàng thành công"
      }
      this.GetShopList().then(() => {this.isHideToast = false}).then(() => {
        this.isHideParent = true;
        setTimeout(() => {
          this.isHideToast = true;
        },3000);
      })
    },
    // 4. 
    async GetShopList(){
      this.isLoading = true;
      await axios.get("https://localhost:44348/api/v1/Shop").then(response => {
        this.shops = response.data.data; 
        this.isLoading = false;
        })
      
    }
  },
  data() {
    return {
      /**
       * isHideParent là trạng thái ẩn hiện Modal Thêm nhân viên
       * selectedEmployee là nhân viên người dùng double click lên
       * employees là mảng chứa các nhân viên ta lấy từ server
       * baseEmployee là Employee trống để phục vụ việc thêm nhân viên mới
       * Header để dùng cho việc router
       */
      isHideParent : true,
      isHidePopup : true,
      isHideToast : true,
      selectedShop : {
        shopAddress: '',
        shopID: '',
        shopName: '',
        shopStatus: 1,
        shopPhoneNumber : '',
        shopCode : '',
        shopTaxCode :'',
        shopNation :"",
        shopStreet: "",
        districtName : "default",
        wardName : "default",
        cityName : "default",
      },

      shops: [],

      popupContent : "Bạn có chắc muốn xóa ",
      popupType: 'danger',
      popupHeader : "Xóa thông tin nhân viên ",
      popupState : '',

      isLoading : false,
      toastContent : 'Thêm mới cửa hàng thành công'
    };
  },
  created() {

    /**
     * Lấy dữ liệu từ Backend 
     * Created By : NHLong
     * Date : 25/01/2021
     */

    this.GetShopList();
    this.preventUncheck();
  },
};
</script>

<style scoped>
/* Styte the header */
.header-content{
  position: relative;
  width : 100%;
}
/* Style the user Avatar */
.grid-employee {
  margin-top: 10px;
  height: calc(100vh - 234px);
}

.el-avatar-employee {
  padding-top: 16px;
  padding-right: 16px;
}

.el-left {
  width: calc(100% - 180px);
}

.el-avatar-employee .el-avatar {
  border: 1px solid #ccc;
  width: 160px;
  height: 160px;
  margin: 0 auto;
  border-radius: 50%;
  cursor: pointer;
  /* background-image: url("/assets/img/default-avatar.jpg"); */
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.el-avatar-note {
  font-size: 12px;
}

/* Style the Fitler bar  */
.filter-left {
  display: flex;
}
.filter-left select {
  margin-left: 10px;
  margin-right: 10px;
}

.currency-for-input {
  position: absolute;
  right: 40px;
  line-height: 40px;
  font-style: italic;
}

#txtSearchEmployee {
  min-width: 300px;
}

.filter-holder{
  display:flex;
  max-height: 50px;
}
.filter-type{
  display:flex; 
  align-items:center;
  justify-content: center;
  min-width:39px;
  border : 1px solid #bbbbbb;
}

.filter-input{
  border-radius: 0 !important;
}

/* Style the tool bars */
.tool-bars{
  display: flex;
  background-color: #2B3173;
  width: 100%;
  height: 34px;
  align-items: center;
  color: #fff;
}
  .tool-bars .tool-holder{
    width: auto;
    display: flex;
    margin-right: 10px;
    height: calc(100% - 8px);
    align-items: center;
    padding: 0px 5px;
    cursor: pointer;
    border: 1px solid #2B3173;
    margin-left: 4px;

  }
    div.tool-holder:hover{
      border : 1px solid #fff;
    }
      div.icon{
        width: 20px;
        height: 20px;
        margin-right: 10px;
      }
      div.add-icon{
        margin-left: 5px;
        background: url("../../../assets/icon/common-icon.png") no-repeat -150px -202px;
      }
      div.duplicate-icon{
        background: url("../../../assets/icon/common-icon.png") no-repeat -972px -126px;
      }
      div.delete-icon{
        background: url("../../../assets/icon/common-icon.png") no-repeat 4px -51px;
      }
      div.edit-icon{
        background: url("../../../assets/icon/common-icon.png") no-repeat -71px -201px;
      }
      div.reload-icon{
        background: url("../../../assets/icon/common-icon.png") no-repeat -121px -200px;
      }

/* Stytle the Restaurant list table */
div.table-container{
  height: 700px;
  overflow: auto;
}

td{
  padding: 5px 10px 4px 10px;
  text-overflow: ellipsis;
  position: relative;
}
th{
  text-overflow: ellipsis;
  padding: 7px 10px;
}

table,th,td{
  border: 1px solid #ccc;
  border-collapse: collapse;
}

table{
  margin : 10px 0px;
  width: 100%;
  overflow: auto;
  height: 500px;
}
td.text-center{
  display: flex; 
  justify-content:center;
}
tbody tr:nth-child(even) {
  background-color: #eee;
}
tbody tr:nth-child(odd) {
  background-color: #fff;
}
tbody tr:hover{
  background-color: #E2E4F1;
}
input{
  max-height: 40px;
}
input[type=checkbox]{
  align-self: center;
  width: 20px;
}
    tr.selected-row{
      background-color:#E2E4F1  !important;
    }

/* Style the Footer Bar */
.paging-bar{
  position: absolute;
  bottom : 0px;
  width: calc(100% - 284px);
}
  select#record-per-page{
    position: absolute;
    left:325px;
    width: auto;
    height: 35px;
    padding-top : 8px;
    padding-left: 16px;
    padding-right:0px;
  }
</style>