<template>
    <div >
<tr v-for="(invoice_product, k) in invoice_products" :key="k">
    <td scope="row" class="trashIconContainer">
         <v-btn @click="deleteRow(k, invoice_product)" small color="primary">Remove</v-btn>
        <!-- <i class="far fa-trash-alt" @click="deleteRow(k, invoice_product)"></i> -->
    </td>
    <td>
        <v-text-field
            label="Regular"
              type="number"
            v-model="invoice_product.product_no"
          ></v-text-field>
        <!-- <input class="form-control" type="text" v-model="invoice_product.product_no" /> -->
    </td>
    <td>
        <v-text-field
            label="Regular"
            v-model="invoice_product.product_name"
          ></v-text-field>
        <!-- <input class="form-control" type="text" v-model="invoice_product.product_name" /> -->
    </td>
    <td>
        <v-text-field
            label="Regular"
              type="number"
            v-model="invoice_product.product_price"
           v-on:keyup="calculateLineTotal(invoice_product)"
          ></v-text-field>
        <!-- <input class="form-control text-right" type="number" min="0" step=".01" v-model="invoice_product.product_price" @change="calculateLineTotal(invoice_product)"
        /> -->
    </td>
    <td>
        <v-text-field
            label="Regular"
              type="number"
           v-model="invoice_product.product_qty"
           v-on:keyup="calculateLineTotal(invoice_product)"
          ></v-text-field>
        <!-- <input class="form-control text-right" type="number" min="0" step=".01" v-model="invoice_product.product_qty" @change="calculateLineTotal(invoice_product)"
        /> -->
    </td>
    <td>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="invoice_product.line_total"
            label="Regular"
            readonly
          ></v-text-field>
        </v-col>
        <!-- <input readonly class="form-control text-right" type="number" min="0" step=".01" v-model="invoice_product.line_total" /> -->
    </td>
</tr>
<div>
    <v-btn @click="addNewRow" small color="primary">Add</v-btn>
    <!-- <button type='button' class="btn btn-info" @click="addNewRow">
    <i class="fas fa-plus-circle"></i>
    Add
</button> -->
</div>
 <v-card class="mx-auto" max-width="344">
      <v-card-text>
        <div>{{invoice_products}}</div>
      </v-card-text>
      <v-card-actions>
        <v-btn text color="deep-purple accent-4">Learn More</v-btn>
      </v-card-actions>
    </v-card>
</div>

</template>


<script>
export default {
    data: () => ({
            invoice_products: [{
                product_no: '',
                product_name: '',
                product_price: '',
                product_qty: '',
                line_total: 0
            }]
        }),
   
    

methods: {
        saveInvoice() {
            console.log(JSON.stringify(this.invoice_products));
        },
        calculateTotal() {
            var subtotal, total;
            subtotal = this.invoice_products.reduce(function (sum, product) {
                var lineTotal = parseFloat(product.line_total);
                if (!isNaN(lineTotal)) {
                    return sum + lineTotal;
                }
            }, 0);

            this.invoice_subtotal = subtotal.toFixed(2);

            total = (subtotal * (this.invoice_tax / 100)) + subtotal;
            total = parseFloat(total);
            if (!isNaN(total)) {
                this.invoice_total = total.toFixed(2);
            } else {
                this.invoice_total = '0.00'
            }
        },
        calculateLineTotal(invoice_product) {
            var total = parseFloat(invoice_product.product_price) * parseFloat(invoice_product.product_qty);
            if (!isNaN(total)) {
                invoice_product.line_total = total.toFixed(2);
            }
            this.calculateTotal();
        },
        deleteRow(index, invoice_product) {
            var idx = this.invoice_products.indexOf(invoice_product);
            console.log(idx, index);
            if (idx > -1) {
                this.invoice_products.splice(idx, 1);
            }
            this.calculateTotal();
        },
        addNewRow() {
            this.invoice_products.push({
                product_no: '',
                product_name: '',
                product_price: '',
                product_qty: '',
                line_total: 0
            });
        }
}

}
</script>
