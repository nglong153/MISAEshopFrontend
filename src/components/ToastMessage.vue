<template>
    <div v-show="!isHideToast" class="toast">
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
    right: 0px;
    top: 0px;
}

/* Style Toast Message display */
.toast {
    /* Style height and width */
    height: 40px;
    width: 300px;
    
    /* Style text */
    line-height: 40px;
    text-align: center;
    color: white;
    /* Style background */
    border-radius: 4px;
    background-color: #009933;
    border: none;
}

/* class Show to display the toast */
.show{
    display:block;
}
</style>