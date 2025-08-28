<template>
  <div class="home-page">
    <!-- 轮播图区域 -->
    <div class="carousel-section" style="height: 600px">
      <pet-carousel :breeds="silderList"></pet-carousel>
    </div>
    <!-- 可视化图表区域 -->
    <div class="section-container">
      <div class="section-header">
        <h2>可视化</h2>
        <div class="section-divider"></div>
      </div>
      <div class="section-content">
        <showViewVue></showViewVue>
      </div>
    </div>
    <!-- 宠物推荐区域 -->
    <div class="section-container">
      <div class="section-header">
        <h2>宠物推荐</h2>
        <div class="section-divider"></div>
      </div>
      <div class="section-content">
        <pet-showcase :pets="pets"></pet-showcase>
      </div>
    </div>

    <!-- 宠物用品区域 -->
    <div class="section-container">
      <div class="section-header">
        <h2>宠物用品</h2>
        <div class="section-divider"></div>
      </div>
      <div class="section-content">
        <pet-products :products="products"></pet-products>
      </div>
    </div>
  </div>
</template>
  
  <script>
import PetCarousel from "../../../components/front/PetCarousel.vue";
import PetShowcase from "../../../components/front/PetShowcase.vue";
import PetProducts from "../../../components/front/PetProducts.vue";
import Request from "../../../utils/request.js";
import showViewVue from "./showView.vue";
export default {
  components: { PetCarousel, PetShowcase, PetProducts, showViewVue },
  mounted() {
    this.loadPets(); // 获取物数据
    this.loadProducts(); // 获取物品数据
    this.loadSlider(); // 获取轮播数据
    window.addEventListener('wheel', this.handleWheel, { passive: false });
  },
  beforeDestroy() {
    window.removeEventListener('wheel', this.handleWheel);
  },
  data() {
    return {
      pets: [],
      products: [],
      silderList: [],
      breeds: [
        {
          id: 1,
          name: "拉布拉多",
          desrc: "温和友善、聪明易训练",
          img: "/img/testImg/1.png",
        },
        {
          id: 2,
          name: "金吉拉",
          desrc: "波斯猫的选育后",
          img: "/img/testImg/2.jpeg",
        },
        {
          id: 3,
          name: "豹纹守宫",
          desrc: "温顺性格和易于照料",
          img: "/img/testImg/3.jpeg",
        },
      ],
    };
  },
  methods: {
    handleWheel(e) {
      e.preventDefault();
      e.stopPropagation();
      
      const delta = Math.sign(e.deltaY);
      const windowHeight = window.innerHeight;
      const docHeight = document.documentElement.scrollHeight;
      const currentScroll = window.scrollY;
      
      let targetScroll;
      if (delta > 0) { // 向下滚动
        targetScroll = Math.min(
          currentScroll + windowHeight,
          docHeight - windowHeight
        );
      } else { // 向上滚动
        targetScroll = Math.max(
          currentScroll - windowHeight,
          0
        );
      }
      
      // 只有当目标位置与当前位置不同时才滚动
      if (Math.abs(targetScroll - currentScroll) > 10) {
        window.scrollTo({
          top: targetScroll,
          behavior: 'smooth'
        });
      }
    },
    
    loadSlider() {
      Request.get("/slider/selectAll").then((res) => {
        if (res.code == 0) {
          this.silderList = res.data;
          console.log("silderList" + this.silderList);
        }
      });
    },
    loadPets() {
      Request.get("/animal/selectAll").then((res) => {
        if (res.code == 0) {
          this.pets = res.data;
        } else {
          // console.log('获取物数据失败');
        }
      });
    },
    loadProducts() {
      Request.get("/goods/selectAll").then((res) => {
        if (res.code == 0) {
          this.products = res.data;
        } else {
          // console.log('获取物数据失败');
        }
      });
    },
  },
};
</script>
  
  <style scoped>
.home-page {
  display: flex;
  width: 100%;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #e3f2dd;
  padding-bottom: 40px;
  background-image: url('../../../assets/猫狗双全.png');
  background-size: 300px;
  background-repeat: no-repeat;
  background-position: right bottom;
  background-attachment: fixed;
  background-blend-mode: overlay;
}

.carousel-section {
  width: 100%;
  margin-bottom: 30px;
  padding: 20px 0;
}

.section-container {
  width: 90%;
  max-width: 1200px;
  margin-bottom: 60px;
  position: relative;
}

.section-container::before {
  content: '';
  position: absolute;
  top: -20px;
  left: 0;
  right: 0;
  height: 20px;
  background-image: url('../../../assets/逗狗宠物.svg');
  background-repeat: repeat-x;
  background-size: contain;
  opacity: 0.6;
}

.section-header {
  display: flex;
  align-items: center;
  margin-bottom: 30px;
  position: relative;
}

.section-header h2 {
  font-size: 28px;
  font-weight: 600;
  color: #7dad69;
  margin: 0;
  padding: 0 20px;
  position: relative;
  z-index: 1;
  background-color: #fff9f9;
  border-radius: 50px;
  box-shadow: 0 4px 15px rgba(255, 133, 162, 0.2);
}

.section-divider {
  flex: 1;
  height: 3px;
  background: linear-gradient(to right, #ffb6c1, #ffd3b6);
  margin-left: 10px;
  border-radius: 3px;
}

.section-content {
  background-color: white;
  border-radius: 20px;
  padding: 25px;
  box-shadow: 0 5px 20px rgba(255, 182, 193, 0.15);
  border: 2px solid rgba(255, 214, 221, 0.8);
  transition: transform 0.3s ease;
}

.section-content:hover {
  transform: translateY(-5px);
}

@media (max-width: 768px) {
  .carousel-section {
    padding: 10px 0;
  }

  .section-container {
    width: 95%;
  }

  .section-header h2 {
    font-size: 20px;
  }

  .section-content {
    padding: 15px;
  }
}
</style>
