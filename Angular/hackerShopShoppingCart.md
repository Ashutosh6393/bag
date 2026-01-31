###app.component.html
 
 
<h8k-navbar header="Hacker Shop"></h8k-navbar>
 
<div class="layout-row shop-component">
  <app-product-list [products]="products"
                    (onAddToCart)="addToCart($event)"
                    (onQuantityUpdate)="updateCart($event)"
                    (onQuantityDecrease)="decreaseCart($event)"
                    class="flex-70"></app-product-list>
  <app-cart class="flex-30"
            [cart]="cart"></app-cart>
</div>
 
 
 
 
###app.component.tS
import {Component} from '@angular/core';
import {Cart, Product} from "../types";
 
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})
export class AppComponent {
  products: Product[];
  cart: Cart;
 
  constructor() {
    this.cart = {
      items: []
    } as Cart
  }
 
  ngOnInit() {
    this.products = [...PRODUCTS].map((product, index) => {
      product.id = index + 1;
      product.image = `/assets/images/items/${product.name.toLocaleLowerCase()}.png`;
      product.cartQuantity = 0;
      return product;
    });
  }
 
  addToCart(product: Product) {
     const productInList = this.products.find(p => p.id === product.id);
     if (productInList) {
       productInList.cartQuantity = 1;
       this.cart.items.push({id: product.id, item: product.name, quantity: 1});
     }
  }
 
  updateCart(product: Product) {
      const UpdateQunatity = this.products.find(p=>p.id === product.id);
      const cartItem = this.cart.items.find(item => item.id === product.id);
      if (UpdateQunatity && cartItem) {
        UpdateQunatity.cartQuantity += 1;
        cartItem.quantity += 1;
      }
  }
 
  decreaseCart(product: Product) {
      const productInList = this.products.find(p => p.id === product.id);
      const cartItemIndex = this.cart.items.findIndex(item => item.id === product.id);
      if (productInList && cartItemIndex !== -1) {
        productInList.cartQuantity -= 1;
        if (productInList.cartQuantity === 0) {
          this.cart.items.splice(cartItemIndex, 1);
        } else {
          this.cart.items[cartItemIndex].quantity -= 1;
        }
      }
  }
}
 
 
export const PRODUCTS: Product[] = [
  {
    name: "Cap",
    price: 5
  },
  {
    name: "HandBag",
    price: 30
  },
  {
    name: "Shirt",
    price: 35
  },
  {
    name: "Shoe",
    price: 50
  },
  {
    name: "Pant",
    price: 35
  },
  {
    name: "Slipper",
    price: 25
  }
];
 
 
### product-list.component.html
 
<div class="layout-row wrap justify-content-center">
  <section *ngFor="let product of products; let idx = index;" class="w-30 product-item">
    <div class="card ma-16">
      <img [src]="product.image" class="product-image"/>
      <div class="card-text pa-4">
        <h5 class="ma-0 text-center">{{product.name}}</h5>
        <p class="ma-0 mt-8 text-center">${{product.price}}</p>
      </div>
      <div class="card-actions justify-content-center pa-4">
        <button *ngIf="product.cartQuantity === 0" (click)="onAdd(product)"class="x-small outlined" data-test-id="btn-item-add">
          Add To Cart
        </button>
 
        <div *ngIf="product.cartQuantity > 0" class="layout-row justify-content-between align-items-center">
          <button (click)="onDecrement(product)" class="x-small icon-only outlined"
                  data-test-id="btn-quantity-subtract">
            <i class="material-icons">remove</i>
          </button>
 
          <input type="number" class="cart-quantity" data-test-id="cart-quantity" [value]="product.cartQuantity"/>
 
          <button (click)=" onIncrement(product)" class="x-small icon-only outlined"
                  data-test-id="btn-quantity-add">
            <i class="material-icons">add</i>
          </button>
        </div>
      </div>
    </div>
  </section>
</div>
 
 
 
###  product-list.component.ts
<div class="layout-row wrap justify-content-center">
  <section *ngFor="let product of products; let idx = index;" class="w-30 product-item">
    <div class="card ma-16">
      <img [src]="product.image" class="product-image"/>
      <div class="card-text pa-4">
        <h5 class="ma-0 text-center">{{product.name}}</h5>
        <p class="ma-0 mt-8 text-center">${{product.price}}</p>
      </div>
      <div class="card-actions justify-content-center pa-4">
        <button *ngIf="product.cartQuantity === 0" (click)="onAdd(product)"class="x-small outlined" data-test-id="btn-item-add">
          Add To Cart
        </button>
 
        <div *ngIf="product.cartQuantity > 0" class="layout-row justify-content-between align-items-center">
          <button (click)="onDecrement(product)" class="x-small icon-only outlined"
                  data-test-id="btn-quantity-subtract">
            <i class="material-icons">remove</i>
          </button>
 
          <input type="number" class="cart-quantity" data-test-id="cart-quantity" [value]="product.cartQuantity"/>
 
          <button (click)=" onIncrement(product)" class="x-small icon-only outlined"
                  data-test-id="btn-quantity-add">
            <i class="material-icons">add</i>
          </button>
        </div>
      </div>
    </div>
  </section>
</div>