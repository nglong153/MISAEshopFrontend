<template>
    <div class="navbar">
        <div class="navbar-content">
            <a class="nav-item">
                <div class="nav-item-icon nav-item-dashboard"></div>
                <div class="nav-item-text">Tổng quan</div>
            </a>
            <a class="nav-item">
                <div class="nav-item-icon nav-item-report"></div>
                <div class="nav-item-text">Báo cáo</div>
            </a>
            <a href="" class="nav-item">
                <div class="nav-item-icon nav-item-order"></div>
                <div class="nav-item-text">Đơn Hàng</div>
            </a>
            <a class="nav-item">
                <div class="nav-item-icon nav-item-purchase"></div>
                <div class="nav-item-text">Mua hàng</div>
            </a>
            <!-- <a class="nav-item" :href="customer">
                <div class="nav-item-icon nav-item-dic"></div>
                <div class="nav-item-text">Danh mục khách hàng</div>
            </a> -->
            <router-link class="nav-item" to="/dictionary/customer">
                <div class="nav-item-icon nav-item-sale"></div>
                <div class="nav-item-text">Khuyến mãi</div>
            </router-link>
             <router-link class="nav-item" to="/dictionary/shops">
                <div class="nav-item-icon nav-item-store-manager"></div>
                <div class="nav-item-text">Quản lý cửa hàng</div>
            </router-link>
            <a class="nav-item">
                <div class="nav-item-icon nav-item-goods"></div>
                <div class="nav-item-text">Hàng hóa</div>
            </a>
        </div>
    </div>
</template>
<script>
export default {
  name: 'MenuBarLinks',
  data () {
    return {
      customer: '/dictionary/customer',
      employee: '/dictionary/employee'
    };
  }
};
</script>
