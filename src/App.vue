<template>
  <div class="container-fluid">
    <nav class="navbar px-4 navbar-expand-lg navbar-dark bg-primary">
      <a class="navbar-brand" href="#">Navbar</a>
    </nav>
    <div class="row mt-4">
      <div class="col-12 col-md-4 form-group">
        <label for="category" class="d-inline-block">Categorías: </label>
        <select id="category" class="form-control" @change="setSelectedCategory($event)">
          <option v-for="(category, index) in categories" :key="index" :value="category.id">{{category.name}}</option>
        </select>
      </div>
      <div class="col-12 col-md-4 form-group">
        <label for="products">Productos: </label>
        <select id="products" class="form-control" @change="setSelectedProduct($event)">
          <option v-for="(product, index) in displayedProducts" :key="index" :value="product.id">{{product.name}}</option>
        </select>
      </div>
      <div class="col-12 col-md-4 form-group">
        <label for="brands">Marcas: </label>
        <select id="brands" class="form-control" @change="setSelectedBrand($event)">
          <option v-for="(brand, index) in displayedBrands" :key="index" :value="brand.id">{{brand.name}}</option>
        </select>
      </div>
    </div>
    <div class="row mt-4">
    </div>
    <div class="mt-4">
      <div class="col-12 text-center">
        <h3>Sales by moth for: {{getSelectedBrandName()}}</h3>
      </div>
    </div>
    <div class="row mt-4">
      <div class="col-12" v-if="!isLoading">
        <chart :data="salesData"/>
      </div>
    </div>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
export default {
  name: 'App',
  components: {
    Chart
  },
  data() {
    return {
      categories: [],
      products: [],
      brands: [],
      displayedProducts: [],
      displayedBrands: [],
      selectedCategoryId: null,
      selectedProductId: null,
      selectedBrandId: null,
      salesData: [],
      isLoading: false
    }
  },
  created() {
    this.getValues();
    this.selectedCategoryId = this.categories[0].id;
  },
  mounted() {
  },
  watch: {
    selectedCategoryId: {
      handler: 'setDisplayedProducts',
      deep: true
    },
    selectedProductId: {
      handler: 'setDisplayedBrands',
      deep: true
    },
    selectedBrandId: {
      handler: 'setSalesData',
      deep: true
    }
  },
  methods: {
    getSelectedBrandName() {
      const selectedBrand = this.brands.find((brand) => brand.id === this.selectedBrandId);
      if (selectedBrand) {
        return selectedBrand.name;
      }
    },
    setSelectedCategory(event) {
      this.selectedCategoryId = event.target.value;
    },
    setSelectedProduct(event) {
      this.selectedProductId = event.target.value;
    },
    setSelectedBrand(event) {
      this.selectedBrandId = event.target.value;
    },
    setDisplayedProducts() {
      this.displayedProducts = this.products.filter((product) => product.categoryId === this.selectedCategoryId);
      this.selectedProductId = this.displayedProducts[0].id;
    },
    setDisplayedBrands() {
      this.displayedBrands = this.brands.filter((brand) => brand.productId === this.selectedProductId);
      this.selectedBrandId = this.displayedBrands[0].id;
    },
    setSalesData() {
      this.isLoading = true;
      this.salesData = [];
      const selectedBrand = this.brands.find((brand) => brand.id === this.selectedBrandId);
      this.salesData = selectedBrand.sales.map((sale) => sale.units);
      this.isLoading = false;
    },
    getValues() {
      this.categories = [
        {
          id: '1',
          name: 'Electrónicos'
        },
        {
          id: '2',
          name: 'Ropa'
        },
        {
          id: '3',
          name: 'Comida'
        }
      ];
      this.products = [
        {
          id: '1',
          name: 'Teléfonos',
          categoryId: '1'
        },
        {
          id: '2',
          name: 'Computadoras',
          categoryId: '1'
        },
        {
          id: '3',
          name: 'Televisores',
          categoryId: '1'
        },
        {
          id: '4',
          name: 'Camisetas',
          categoryId: '2'
        },
        {
          id: '5',
          name: 'Pantalones',
          categoryId: '2'
        },
        {
          id: '6',
          name: 'Zapatillas',
          categoryId: '2'
        },
        {
          id: '7',
          name: 'Pizza',
          categoryId: '3'
        },
        {
          id: '8',
          name: 'Hamburguesas',
          categoryId: '3'
        },
        {
          id: '9',
          name: 'Cereales',
          categoryId: '3'
        }
      ];
      this.brands = [
        {
          id: '1',
          name: 'Apple',
          productId: '1',
          sales: [
            {
              month: 'Enero',
              units: 100
            },
            {
              month: 'Febrero',
              units: 150
            },
            {
              month: 'Marzo',
              units: 60
            },
            {
              month: 'Abril',
              units: 89
            }
          ]
        },
        {
          id: '2',
          name: 'Samsung',
          productId: '1',
          sales: [
            {
              month: 'Enero',
              units: 22
            },
            {
              month: 'Febrero',
              units: 567
            },
            {
              month: 'Marzo',
              units: 200
            },
            {
              month: 'Abril',
              units: 250
            }
          ]
        },
        {
          id: '3',
          name: 'Dell',
          productId: '2',
          sales: [
            {
              month: 'Enero',
              units: 55
            },
            {
              month: 'Febrero',
              units: 10
            },
            {
              month: 'Marzo',
              units: 200
            },
            {
              month: 'Abril',
              units: 1000
            }
          ]
        },
        {
          id: '4',
          name: 'Alienware',
          productId: '2',
          sales: [
            {
              month: 'Enero',
              units: 33
            },
            {
              month: 'Febrero',
              units: 150
            },
            {
              month: 'Marzo',
              units: 800
            },
            {
              month: 'Abril',
              units: 900
            }
          ]
        },
        {
          id: '5',
          name: 'Sony',
          productId: '3',
          sales: [
            {
              month: 'Enero',
              units: 600
            },
            {
              month: 'Febrero',
              units: 1350
            },
            {
              month: 'Marzo',
              units: 430
            },
            {
              month: 'Abril',
              units: 200
            }
          ]
        },
        {
          id: '6',
          name: 'Philips',
          productId: '3',
          sales: [
            {
              month: 'Enero',
              units: 120
            },
            {
              month: 'Febrero',
              units: 153
            },
            {
              month: 'Marzo',
              units: 605
            },
            {
              month: 'Abril',
              units: 839
            }
          ]
        },
        {
          id: '7',
          name: 'Tommy',
          productId: '4',
          sales: [
            {
              month: 'Enero',
              units: 125
            },
            {
              month: 'Febrero',
              units: 300
            },
            {
              month: 'Marzo',
              units: 56
            },
            {
              month: 'Abril',
              units: 890
            }
          ]
        },
        {
          id: '8',
          name: 'Polo',
          productId: '4',
          sales: [
            {
              month: 'Enero',
              units: 100
            },
            {
              month: 'Febrero',
              units: 150
            },
            {
              month: 'Marzo',
              units: 60
            },
            {
              month: 'Abril',
              units: 89
            }
          ]
        },
        {
          id: '9',
          name: 'Wangler',
          productId: '5',
          sales: [
            {
              month: 'Enero',
              units: 120
            },
            {
              month: 'Febrero',
              units: 130
            },
            {
              month: 'Marzo',
              units: 600
            },
            {
              month: 'Abril',
              units: 891
            }
          ]
        },
        {
          id: '10',
          name: 'Levis',
          productId: '5',
          sales: [
            {
              month: 'Enero',
              units: 134
            },
            {
              month: 'Febrero',
              units: 145
            },
            {
              month: 'Marzo',
              units: 600
            },
            {
              month: 'Abril',
              units: 895
            }
          ]
        },
        {
          id: '11',
          name: 'Puma',
          productId: '6',
          sales: [
            {
              month: 'Enero',
              units: 123
            },
            {
              month: 'Febrero',
              units: 150
            },
            {
              month: 'Marzo',
              units: 30
            },
            {
              month: 'Abril',
              units: 50
            }
          ]
        },
        {
          id: '12',
          name: 'Converse',
          productId: '6',
          sales: [
            {
              month: 'Enero',
              units: 100
            },
            {
              month: 'Febrero',
              units: 250
            },
            {
              month: 'Marzo',
              units: 400
            },
            {
              month: 'Abril',
              units: 880
            }
          ]
        },
        {
          id: '13',
          name: 'Dominos',
          productId: '7',
          sales: [
            {
              month: 'Enero',
              units: 120
            },
            {
              month: 'Febrero',
              units: 50
            },
            {
              month: 'Marzo',
              units: 605
            },
            {
              month: 'Abril',
              units: 189
            }
          ]
        },
        {
          id: '14',
          name: 'Little Ceasars',
          productId: '7',
          sales: [
            {
              month: 'Enero',
              units: 120
            },
            {
              month: 'Febrero',
              units: 156
            },
            {
              month: 'Marzo',
              units: 607
            },
            {
              month: 'Abril',
              units: 892
            }
          ]
        },
        {
          id: '15',
          name: 'Burger King',
          productId: '8',
          sales: [
            {
              month: 'Enero',
              units: 1023
            },
            {
              month: 'Febrero',
              units: 150
            },
            {
              month: 'Marzo',
              units: 6050
            },
            {
              month: 'Abril',
              units: 895
            }
          ]
        },
        {
          id: '16',
          name: 'McDonnalds',
          productId: '8',
          sales: [
            {
              month: 'Enero',
              units: 110
            },
            {
              month: 'Febrero',
              units: 10
            },
            {
              month: 'Marzo',
              units: 60
            },
            {
              month: 'Abril',
              units: 89
            }
          ]
        },
        {
          id: '17',
          name: 'Kelloggs',
          productId: '9',
          sales: [
            {
              month: 'Enero',
              units: 190
            },
            {
              month: 'Febrero',
              units: 140
            },
            {
              month: 'Marzo',
              units: 602
            },
            {
              month: 'Abril',
              units: 893
            }
          ]
        },
        {
          id: '18',
          name: 'General Mills',
          productId: '9',
          sales: [
            {
              month: 'Enero',
              units: 145
            },
            {
              month: 'Febrero',
              units: 50
            },
            {
              month: 'Marzo',
              units: 623
            },
            {
              month: 'Abril',
              units: 69
            }
          ]
        },
      ];
    }
  }
}
</script>

<style scoped>
</style>
