<template>
  <div class="select-box-container">
    <div class="custom-selectbox" :class="{inverted:selecting}" @click="selecting = !selecting"> 
        <span class="selected">{{defaultOption}}</span>
        <span class="arrow"></span>
    </div>
    <div class="items">
      <div v-for="option in options" 
      :key="option.id"
      class="select-box-option" 
      :class="{hide:!selecting, clicked : option.name == defaultOption}"
      @click="optionSelected(option.id)">
          <span class="select-tick"></span>
          <div class="select-option-name">{{option.name}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'SelectBox',
 
  props: ['options','defaultOption','open','selectBoxId'],
  data(){
    return {
      selectedOption : this.defaultOption,
      selecting : false
    }
  },
  
  methods:{
    optionSelected(OptionId){
      this.selecting = !this.selecting;
      this.$emit('selected',this.selectBoxId,OptionId)
    }
  },
  
  mounted(){
    console.log('mounted')
    var optionList = document.querySelectorAll(".select-box-option")
    optionList.forEach(element => 
    {
      element.addEventListener('click',() => 
      {
        element.classList.add('clicked');
      })
    });
  },
  updated(){
  }
}


</script>

<style scoped>
.select-box-container{
  width: 211px;
  margin-left: 10px;
}
  .custom-selectbox{
      width: 211px;
      height: 38px;
      display: flex;
      border: 1px solid #BBBBBB;
      border-radius: 4px;
      justify-content: space-between;
      background-color: #ffffff;
      -moz-user-select: none;
      -webkit-user-select: none;
      user-select:none;
    }
      .selected{
        padding: 0 0 4px 16px;
        line-height: 38px;
        text-align: center;
        height: 38px;
      }

      .arrow{
        width: 16px;
        justify-self: end;
        padding: 0px 16px;
        border-left: 1px solid #BBBBBB;
        background-image: url('../assets/icon/arrow-down-sign-to-navigate.png');
        background-position: center;
        background-size: 16px 16px;
        background-repeat: no-repeat; 
      }
      .custom-selectbox.inverted .arrow{
        transform: rotate(180deg);
        border-left: 0px;
        background-color: #e5e5e5;
      }
  .custom-selectbox.inverted{
    border: 1px solid #019160;
  }
  /* Style the Select Box option */
  .items{
    z-index: 1;
    background-color: white;
    position: absolute;
  }
    .select-box-option{
      margin-top: 4px;
      width: 213px;
      display: flex;
      height: 40px;
      justify-content: start;
      align-items: center;
      background : white;
    }
    .select-box-option:hover{
      background : #e9ebee;
    }
  /* Style the Tick */
    .select-tick{
      position: relative;
      width: 16px;
      height: 40px;
      padding : 0px 10px;
    }
      .select-tick::after{
        content: "";
        width: 5px;
        height: 10px;
        position: absolute;
        top: 13px;
        left: 14px;
        border: solid white;
        border-width: 0px 3px 3px 0px;
        -webkit-transform: rotate(20deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
      }
    .select-option-name{
      width : calc(100% - 36px);
      height: 40px;
      line-height: 40px;
      color: #000000;
    }
   /*Style the selected option  */
   .select-box-option.clicked{
     background-color: #019160 !important;
     color : #ffffff;
   }
   /* Style when user hover the option */
   .select-box-option:hover{
     background-color: #e9ebee;
   }
  /* No display */
  .hide{
    display: none;
  }
</style>