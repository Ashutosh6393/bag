using Microsoft.AspNetCore.Mvc;

using ShoppingCart.Cart.WebAPI.Data;

using ShoppingCart.Cart.WebAPI.SeedData;

using ShoppingCart.Cart.WebAPI.Services;

using Microsoft.EntityFrameworkCore;
 
namespace ShoppingCart.Cart.WebAPI.Controllers

{

    [ApiController]

    [Route("api/[controller]")]
 
    public class CartController : ControllerBase

    {

        private readonly IShoppingCartService cartService;

        private readonly ShoppingCartContext _context;
 
        public CartController(IShoppingCartService service , ShoppingCartContext context)

        {

            cartService = service;   

            _context = context;

        }
 
        [HttpPost("{id}")]

        public async Task<IActionResult> AddCartItem(int id ,CartItemForm form)

        {

            var item = new CartItem

            {

                CartID = id,

                Name = form.Name,

                Quantity = form.Quantity,

                Price = form.Price

            };

            var cart  = await _context.Carts.FirstOrDefaultAsync(x=> x.CartID == id);

            if (cart == null)

            {

                var mycart = new CartObject

                {

                    CartID = id,

                    TotalAmount = 0,

                };

                mycart.CartItems.Add(item);

                mycart.UpdateCart();

                await _context.Carts.AddAsync(mycart);

                await _context.SaveChangesAsync();

                return Ok();

            }

            cart.CartItems.Add(item);

            cart.UpdateCart();

            await _context.SaveChangesAsync();

            return Ok();

        }
 
        [HttpGet("{id}")]

        public async Task<IActionResult> GetById(int id)

        {

            var Cart  = await _context.Carts.FirstOrDefaultAsync(x => x.CartID == id);

            if (Cart == null)

            {

                return NotFound();

            }

            return Ok(Cart);

        }
 
        [HttpPut("update/{id}")]

        public async Task<IActionResult> UpdateCart(int id, CartItemForm form)

        {

            var cart  = await _context.Carts.FirstOrDefaultAsync(x=> x.CartID == id);

            if(cart == null)

            {

                return NotFound();

            }

            var item = cart.CartItems.FirstOrDefault(x => x.ItemID == form.ItemID);

            if (item  == null)

            {

                return NotFound();

            }

            item.Quantity = form.Quantity;

            cart.UpdateCart();

            await _context.SaveChangesAsync();

            return Ok(); 

        }
 
        [HttpDelete("delete/{id}/{itemId}")]

        public async Task<IActionResult> DeleteCartItem(int id , int itemId)

        {

            var cart = await _context.Carts.FirstOrDefaultAsync(x=> x.CartID==id);

            if (cart == null)

            {

                return NotFound();

            }

            var item = cart.CartItems.FirstOrDefault(x=> x.ItemID == itemId);

            if (item == null)

            {

                return NotFound();

            }

            cart.CartItems.Remove(item);

            cart.UpdateCart();

            await _context.SaveChangesAsync();

            return Ok();

        }
 
        [HttpDelete("clear/{id}")]

        public async Task<IActionResult> DeleteCart(int id)

        {

            var cart = await _context.Carts.FirstOrDefaultAsync(x => x.CartID == id);

            if (cart == null)

            {

                return NotFound();

            }

            cart.CartItems.Clear();

            cart.UpdateCart();

            await _context.SaveChangesAsync();

            return Ok();

        }

    }

}

CartItemForm____need to add a validation
 
using System;

using System.Collections.Generic;

using System.ComponentModel.DataAnnotations;

using System.Linq;

using System.Threading.Tasks;

using Microsoft.AspNetCore.Http;

using Newtonsoft.Json;
 
namespace ShoppingCart.Cart.WebAPI.SeedData

{

    public class CartItemForm

    {

        public int ItemID { get; set; }
 
        public string Name { get; set; }
 
        [Range(1, int.MaxValue)]

        public int Quantity { get; set; }
 
        public float Price { get; set; }

    }

}

 