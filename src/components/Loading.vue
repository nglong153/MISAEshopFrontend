<template>
    <div :class="{isHide : !isLoading}" class="loading-screen"></div>
</template>

<script>
export default {
    props:
    {
        isLoading :{
            type : Boolean,
            required : true,
            default: false
        }
    }
}
</script>

<style scoped>
.loading-screen{
    position: absolute;
    top:0px;
    right:0px;
    width:100%;
    height:100%;
    background-color:#000;
    background-image: url('../assets/img/ajax-loader.gif');
    background-size: 50px;
    background-repeat:no-repeat;
    background-position:center;
    z-index:10000000;
    opacity: 0.4;
    filter: alpha(opacity=40);
}

.isHide{
    display: none !important;
}
</style>