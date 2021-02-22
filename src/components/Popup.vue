<template>
<div>
    <div
      class="m-dialog dialog-detail"
      :class="{isHide : popupState}"
    >
 <div class="dialog-modal" style="z-index:101;"></div>
  <div class="popup" > 
      <div class="popup-header">
          <h2>{{popupHeader}}</h2>
      </div>
      <div class="popup-content">
        <img class="popup-icon" src="../assets/icon/icon-question.png" alt="">
        <p>{{popupContent}}</p>
      </div>
      <div class="popup-footer">
        <Button @click="exeClicked" class="danger popup-footer-action" :class="{ isHide : (popupType !='danger')}">
            <div class="button-icon"></div>
            <span>Xóa</span>
        </Button>
        <Button  class="popup-footer-cancel" @click="cancel">Hủy bỏ</Button>
      </div>
  </div>
  </div>
</div>
</template>

<script>
export default {
    name : "Popup",
    props : {
        /**
         * PopupState : thể hiện trạng thái đóng mở của popup
         */
        popupState:{
            type: Boolean,
            required: true,
            default : false
        },
        /**
         * PopupType : thể hiện loại thông báo (cảnh báo, hay thông báo)
         */
        popupType : {
            type : String,
            required : true,
            default : 'noti'
        },
        /**
         * Popup Header : Thể hiện header của thông báo
         * Example : Xóa bản ghi, Đóng fomr ...
         */
        popupHeader  : {
            type : String,
            required : true,
            default : 'Pop-up thông báo'
        },
        /**
         * Popup Content : Thể hiện nội dung của thông báo
         */
        popupContent : {
            type:String,
            required : true,
            default: 'Đây là 1 thông báo ( cho vui, nàm kảnh)'
        },
        /**
         * Popup Icon : Là đường dẫn đến icon nằm trong thông báo
         */
    },
    methods:{
        cancel(){
            this.$emit('popupClose');
        },
        exeClicked(){
            this.$emit('execute');
        }
    }
    
}
</script>

<style>


button{
    appearance: none;
}

    div.popup{
        min-height: 180px;
        max-width: 450px;
        border-radius: 4px;
        z-index: 101;
        position: fixed;
        top:calc(50vh - 90px);
        left: calc(50% - 225px);
        
    }
    .popup .popup-header{
        width: 100%;
        height: 50px;
        background-color: #e5e5e5;
    }
        .popup .popup-header h2{
            position: absolute;
            top:24px;
            left: 24px;
            font-weight: bold;
            font-size: 15px;
            margin: 0px 24px 24px 0px;
        }   
    .popup .popup-content{
        width: calc(100% - 48px);
        padding: 24px 24px;
        display: flex;
        min-height: calc(100% - 158px);
        background-color: white;
    }
        .popup .popup-content .popup-icon{
            border-radius: 50%;
            width: 48px;
            height: 48px;
            background-color: #e5e5e5;
            margin-right: 10px;
        }

    .popup .popup-footer{
        width: calc(100% -34px);
        height: 60px;
        display: flex;
        padding-left: 10px;
        padding-right: 24px;
        align-items: center;
        justify-content: flex-end;
        background-color: #E9EBEE;
    }
        .popup .popup-footer .popup-footer-action,
        .popup .popup-footer .popup-footer-cancel
        {   
            display: flex;
            align-items: center;
            margin-top: 10px;
            margin-bottom: 10px;
            margin-left: 16px;
            height: 40px;
            border: none;
            border-radius: 4px;
            outline: none;
            cursor: pointer;
            background-color: #f65454;   
            min-width: 90px;
            padding : 0px 10px;
        }
        
        .popup .popup-footer .popup-footer-action:hover{
            background-color: #2fbe8e;
        }

        /* .popup .popup-footer .popup-footer-action::after{
            content: 'Đóng';
            color: white;
        } */

            .popup .popup-footer .popup-footer-action.danger div.button-icon{
                background: url("../assets/icon/common-icon.png") no-repeat -2px -51px;
                height: 20px;
                width:20px;
                background-repeat: no-repeat;
                background-color: #f65454;
            }
            
            .popup .popup-footer .popup-footer-action.danger span{
                color: white;
                width: 30px;
            }

            .popup .popup-footer .popup-footer-action.danger:hover{
                background-color: #f65454;
            }

        .popup .popup-footer .popup-footer-cancel{
            background-color: #E9EBEE;
            border: none;
        }
        .popup .popup-footer .popup-footer-cancel:hover{
            border: 1px solid #bbbbbb;
            border-radius: 4px;
            background-color: #bebebe;
        }
.isHide{
    display: none !important;
}
</style>