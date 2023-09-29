<template>
  <div class="container">
    <div
      :class="{
        'bg-blue container': isMensClothing,
        'bg-pink container': isWomensClothing,
      }"
    >
      <div class="overlay">
        <img src="../assets/bg-pattern.svg" alt="background overlay" />
      </div>
      <div class="card" v-if="productData.length > 0">
        <div class="product-container">
          <div class="product-thumbnail">
            <img :src="activeProduct.image" alt="" />
          </div>
          <div class="product-details">
            <div class="top">
              <h3
                :class="{
                  'font-navy title': isMensClothing,
                  'font-magenta title': isWomensClothing,
                }"
              >
                {{ activeProduct.title }}
              </h3>
              <div class="sub-title">
                <span>{{ activeProduct.category }} </span>
                <div class="rating">
                  <span>{{ activeProduct.rating.rate }}/5</span>
                  <div class="rating">
                    <span
                      :class="{
                        'bg-navy circle': isMensClothing,
                        'bg-magenta circle': isWomensClothing,
                      }"
                    ></span>
                    <span
                      :class="{
                        'bg-navy circle': isMensClothing,
                        'bg-magenta circle': isWomensClothing,
                      }"
                    ></span>
                    <span
                      :class="{
                        'bg-navy circle': isMensClothing,
                        'bg-magenta circle': isWomensClothing,
                      }"
                    ></span>
                    <span
                      :class="{
                        'bg-navy circle': isMensClothing,
                        'bg-magenta circle': isWomensClothing,
                      }"
                    ></span>
                    <span
                      :class="{
                        'bg-navy circle': isMensClothing,
                        'bg-magenta circle': isWomensClothing,
                      }"
                    ></span>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>
                  {{ activeProduct.description }}
                </p>
              </div>
            </div>
            <div class="bottom">
              <span
                :class="{
                  'font-navy price': isMensClothing,
                  'font-magenta price': isWomensClothing,
                }"
                >${{ activeProduct.price }}</span
              >
              <div class="cta">
                <button
                  type="button"
                  :class="{
                    'bg-navy cta-buy': isMensClothing,
                    'bg-magenta cta-buy': isWomensClothing,
                  }"
                  @click="showBuyNowAlert"
                >
                  Buy Now
                </button>
                <button
                  type="button"
                  :class="{
                    'border-navy cta-next': isMensClothing,
                    'border-magenta cta-next': isWomensClothing,
                  }"
                  @click="nextProduct"
                  :disabled="showLoader"
                >
                  Next Product
                </button>
                <!-- animasi loader -->
                <div v-if="showLoader" class="loader-container">
                  <div class="loader"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      productData: [], //Inisialisasi product
      currentProductIndex: 0, //Indeks Product yang aktif
      showLoader: false,
    };
  },
  methods: {
    // Fungsi untuk memanggil API
    async callProductApi() {
      const api = await fetch("https://fakestoreapi.com/products");
      const response = await api.json();

      // Filter data berdasarkan kategori
      this.productData = response.filter(
        (product) =>
          product.category === "men's clothing" ||
          product.category === "women's clothing"
      );
    },
    // Pesan Alert
    showBuyNowAlert() {
      alert("Maaf, fitur ini belum tersedia");
    },
    // Fungsi untuk mengganti ke card berikutnya
    nextProduct() {
      if (this.currentProductIndex < this.productData.length - 1) {
        this.showLoader = true; //Menampilkan loader

        setTimeout(() => {
          this.currentProductIndex++;
          this.callProductApi()
            .then(() => {
              this.showLoader = false;
            })
            .catch((error) => {
              this.showLoader = false;
              console.error("Error fetching product:", error);
            });
        }, 800); // Tunggu 0,8 detik sebelum mengambil data berikutnya
      } else {
        // Kembali ke data yang paling awal jika sudah mencapai indeks terakhir
        this.currentProductIndex = 0;
        this.showLoader = true;
        setTimeout(() => {
          this.callProductApi()
            .then(() => {
              this.showLoader = false;
            })
            .catch((error) => {
              this.showLoader = false;
              console.error("Error fetching product:", error);
            });
        }, 800);
      }
    },
  },
  mounted() {
    this.callProductApi()
      .then(() => {
        this.showLoader = false;
      })
      .catch((error) => {
        this.showLoader = false;
        console.error("Error fetching product:", error);
      });
  },
  computed: {
    activeProduct() {
      return this.productData[this.currentProductIndex] || {};
    },
    isMensClothing() {
      return this.activeProduct.category === "men's clothing";
    },
    isWomensClothing() {
      return this.activeProduct.category === "women's clothing";
    },
  },
};
</script>

<style src="../assets/style/page.css"></style>
