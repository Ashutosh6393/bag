### app.component.ts
 
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
      items: [],
      subTotal: 0,
      totalPrice: 0,
      discount: 0,
      selectedCoupon: '0'
    } as Cart
  }
 
  ngOnInit() {
    this.products = PRODUCTS.map((product, index) => {
      product.id = index + 1;
      product.image = `/assets/images/items/${product.name.toLocaleLowerCase()}.png`;
      product.cartQuantity = 0;
      return product;
    });
  }
 
  addToCart(product: Product) {
    const index = this.products.findIndex(p => p.id === product.id);
    this.products[index].cartQuantity = 1;
    this.cart.items.push({
      id: this.products[index].id,
      item: this.products[index].heading,
      price: this.products[index].price,
      quantity: 1
    });
    this.updateCart();
  }
 
  removeFromCart(product: Product) {
    const index = this.products.findIndex(p => p.id === product.id);
    this.products[index].cartQuantity = 0;
    const cartIndex = this.cart.items.findIndex(c => c.id === product.id);
    this.cart.items.splice(cartIndex, 1);
    this.updateCart();
  }
 
  updateCoupon(coupon) {
this.cart.selectedCoupon = coupon;
this.updateCart();
  }
 
  updateCart() {
this.cart.subTotal = this.cart.items.reduce((sum,item)=>sum + item.price,0);
this.cart.discount = (this.cart.subTotal * +(this.cart.selectedCoupon) )/100;
this.cart.totalPrice = this.cart.subTotal - this.cart.discount;
  }
}
 
 
export const PRODUCTS: Product[] = [
  {
    heading: "Cap - $10",
    name: "Cap",
    price: 10
  },
  {
    heading: "Hand Bag - $30",
    name: "HandBag",
    price: 30
  },
  {
    heading: "Shirt - $30",
    name: "Shirt",
    price: 30
  },
  {
    heading: "Shoes - $50",
    name: "Shoe",
    price: 50
  },
  {
    heading: "Pant - $40",
    name: "Pant",
    price: 40
  },
  {
    heading: "Slipper - $20",
    name: "Slipper",
    price: 20
  }
];
 
 
 
###cart.component.html
 
<div class="card my-16 mr-25 outlined">
  <section class="layout-row align-items-center justify-content-center px-16">
    <h4>Your Cart</h4>
  </section>
  <div class="divider"></div>
  <section>
    <table>
      <thead>
      <tr>
        <th>Item</th>
        <th>Quantity</th>
        <th class="numeric">Price</th>
      </tr>
      </thead>
      <tbody>
      <tr *ngFor="let cartItem of cart?.items; let idx = index;" id="cart-item-{{idx}}">
        <td data-test-id="cart-item-name">{{cartItem.item}}</td>
        <td data-test-id="cart-item-quantity">{{cartItem.quantity}}</td>
        <td class="numeric" data-test-id="cart-item-price">${{cartItem.price}}</td>
      </tr>
      </tbody>
    </table>
 
    <div class="layout-row justify-content-between align-items-center px-8 mx-12">
      <h5>Select Coupon</h5>
      <select (change)="onCoupon($event)" data-test-id="cart-coupon"
              class="coupon-select">
        <option [value]="0">None</option>
        <option [value]="10">OFF10</option>
        <option [value]="20">OFF20</option>
      </select>
    </div>
    <ul class="bordered inset ma-0 px-8 mt-30">
      <li class="layout-row justify-content-between py-12 caption font-weight-light">
        <span>Subtotal</span>
        <span data-test-id="cart-subtotal">${{cart.subTotal}}</span>
      </li>
      <li class="layout-row justify-content-between py-12 caption font-weight-light">
        <span>Discount (-)</span>
        <span class="discount" data-test-id="cart-discount">${{cart.discount}}</span>
      </li>
      <li class="layout-row justify-content-between py-12 font-weight-bold">
        <span>Total</span>
        <span data-test-id="cart-total">${{cart.totalPrice}}</span>
      </li>
    </ul>
 
  </section>
</div>
 
 
 
### cart.component.ts
 
import {Component, EventEmitter, Input, OnInit, Output} from '@angular/core';
import {Cart} from "../../types";
 
@Component({
  selector: 'app-cart',
  templateUrl: './cart.component.html',
  styleUrls: ['./cart.component.scss'],
  standalone: false
})
export class CartComponent implements OnInit {
  @Input() cart: Cart;
  @Output() onCouponChanged: EventEmitter<string> = new EventEmitter<string>();
 
  ngOnInit() {
  }
     onCoupon(value){
      this.onCouponChanged.emit(value.target.value);
      console.log(value.target.value);
     }
}
 
 
 
 
###product-list-component.ts
<div class="layout-row wrap justify-content-center">
  <section *ngFor="let product of products; let idx = index;" class="w-30 product-item">
    <div class="card ma-16">
      <img [src]="product.image" class="product-image" />
      <div class="card-text pa-4">
        <h5 class="ma-0 text-center">{{product.name}}</h5>
        <p class="ma-0 mt-8 text-center">${{product.price}}</p>
      </div>
      <div class="card-actions justify-content-center pa-4">
        <button *ngIf="product.cartQuantity === 0" (click)="onAdd(product)" class="x-small outlined"
          data-test-id="btn-item-add">
          Add To Cart
        </button>
        <button *ngIf="product.cartQuantity === 1" (click)="onRemove(product)" class="x-small danger" data-test-id="btn-item-remove">
          Remove From Cart
        </button>
      </div>
    </div>
  </section>
</div>
 
 
 
### product-list-component.ts
 
import {Component, EventEmitter, Input, OnInit, Output} from '@angular/core';
import {Product} from "../../types";
 
@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.scss'],
  standalone: false
})
export class ProductListComponent implements OnInit {
  @Input() products: Product[];
  @Output() onAddToCart: EventEmitter<Product> = new EventEmitter();
  @Output() onRemoveFromCart: EventEmitter<Product> = new EventEmitter();
 
  ngOnInit() {
  }
  onAdd(Product){
    this.onAddToCart.emit(Product);
  }
  onRemove(Product){
    this.onRemoveFromCart.emit(Product);
  }
 
}