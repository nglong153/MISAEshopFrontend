<template>
    <div :class="{show : !isHideToast}" class="toast">
        <span>
            {{toastContent}}
        </span>
    </div>
</template>

<script>
export default {
    name : 'ToastMessage',
    props : {
        toastContent : {
            type: String,
            required : true
        },
        isHideToast : {
            type: Boolean,
            required : true
        }
    },
    methods :{
        displayToast(){
            document.querySelector(".toast").classList.add("show")
        },
        hideToast(){
            document.querySelector(".toast").classList.remove("show")
        }
    }

}
</script>

<style scoped>
/* Style Toast Message Position */
.toast{
    position: absolute;
    right: calc(50% - 180px);
    top: 30px;
}

/* Style Toast Message display */
.toast {
    /* Style height and width */
    visibility: hidden;
    height: 80px;
    width: 300px;
    
    /* Style text */
    line-height: 80px;
    text-align: center;
    color: white;
    /* Style background */
    border-radius: 4px;
    background-color: #1aaa4a;
    border: none;
}

/* class Show to display the toast */
.show{

    visibility: visible;
    -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
    animation: fadein 0.5s, fadeout 0.5s 2.5s;
}

/* Animations to fade the snackbar in and out */

@-webkit-keyframes fadein {
  from {top: 0; opacity: 0;}
  to {top: 30px; opacity: 1;}
}

@keyframes fadein {
  from {top: 0; opacity: 0;}
  to {top: 30px; opacity: 1;}
}

@-webkit-keyframes fadeout {
  from {top: 30px; opacity: 1;}
  to {top: 0; opacity: 0;}
}

@keyframes fadeout {
  from {top: 30px; opacity: 1;}
  to {top: 0; opacity: 0;}
}
</style>