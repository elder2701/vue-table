<template>
  <div class="modal-backdrop">
    <div class="modal-product">
      <div class="modal-header">
        <span>Edit</span>
        <div class="btn-close" @click="hadleResetInputs">X</div>
      </div>
      <div class="modal-body">
        <form @submit.prevent="handleUdpateAndResetInputs">
          <div>
            <label for="product">Product Name</label>
            <input v-model.trim="product" type="text" id="product" name="product" required />
          </div>
          <div class=input-container>
            <label for="price">Unit Price</label>
            <input
              v-model="price"
              @blur="checkForm()"
              type="number"
              id="price"
              name="price"
              step="0.1"
              min="0.0"
              required
            />
          </div>
          <div class=input-container>
            <label for="stock">Units in Stock</label>
            <input
              v-model="units"
              @blur="checkForm()"
              type="number"
              id="stock"
              name="stock"
              step="1"
              min="0"
              required
            />
          </div>
          <div>
            <label for="discontinued">Units in Stock</label>
            <input v-model="discontinued" type="checkbox" id="discontinued" name="discontinued" />
          </div>
          <div class="form-button input-container">
            <input type="submit" value="Update" />
            <input type="button" value="Cancel" @click="hadleResetInputs" />
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      product: "",
      price: 0.0,
      units: 0,
      discontinued: true
    };
  },
  props: {
    selectedRow: Object
  },
  watch: {
    /*Устанавливаем watcher за Props`ми. Это необходимо для того, 
    чтобы установить данные для input`ов в режиме редактирования строки*/
    selectedRow: {
      handler: function() {
        if (Object.keys(this.selectedRow).length) {
          this.product = this.selectedRow.product;
          this.price = this.selectedRow.price;
          this.units = this.selectedRow.units;
          this.discontinued = this.selectedRow.discontinued;
        } else {
          this.resetAllField();
        }
      },
      immediate: true
    }
  },
  methods: {
    handleUdpateAndResetInputs() {
      const newData = {
        product: this.product,
        price: this.price,
        units: this.units,
        discontinued: this.discontinued
      };
      console.log(newData)
      this.$emit("on-update-data", { newData });
      this.resetAllField();
    },
    hadleResetInputs() {
      this.$emit("on-close");
    },
    resetAllField() {
      this.product = "";
      this.price = 0.0;
      this.units = 0;
      this.discontinued = false;
    },
    checkForm() {
      /*Обрабатывает некорректные числа, которые начинаются с нулей(например: 01, 01.1 - нули обрезаются), из input type=number.*/
      this.price = +this.price;
      this.units = +this.units;
    }
  }
};
</script>

<style>
.btn-close:hover {
  cursor: default;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-product {
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  display: flex;
  flex-direction: column;
}

.modal-header {
  background-color: rgba(241, 238, 238, 0.979);
  display: flex;
  justify-content: space-between;
}

.modal-header span {
  margin: 5px;
}

.input-container {
  display: flex;
  justify-content: space-between;
}
.btn-close {
  margin: 5px;
}
</style>
