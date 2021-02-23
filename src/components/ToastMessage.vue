<template>
    <div class="toast">
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
        }
    },
    methods :{
        displayToast(){
            document.querySelector(".toast").classList.add("show")
        },
        hideToast(){
            document.querySelector(".toast").classList.remove("show")
        }
    },
    updated(){
        console.log('toast created')
        setTimeout(this.hideToast,2000);
    }

}
</script>

<style scoped>
/* Style Toast Message Position */
.toast{
    display: none;
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