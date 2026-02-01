//CartController
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using ShoppingCart.Cart.WebAPI.Data;
using ShoppingCart.Cart.WebAPI.SeedData;
using ShoppingCart.Cart.WebAPI.Services;

namespace ShoppingCart.Cart.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]

    public class CartController : ControllerBase
    {
        private readonly IShoppingCartService cartService;
        ShoppingCartContext _context;
        public CartController(IShoppingCartService service, ShoppingCartContext con)
        {
            cartService = service;
            _context = con;
        }
        [HttpGet("{id}")]
        public async Task<IActionResult> Get(int id) {
            var data =await _context.Carts.FindAsync(id);
            return Ok(data);
        }
        [HttpPost("{cartId}")]
        public async Task<IActionResult> Addcart(int cartId, [FromBody] CartItem item)
        {
            if (item.Quantity <= 0)
            {
                return BadRequest();
            }
            var cart = await _context.Carts.FindAsync(cartId);
            if (cart == null)
            {
                cart = new CartObject()
                {
                    CartID = cartId,
                };
                _context.Carts.Add(cart);
                await _context.SaveChangesAsync();
            }
            cart.CartItems.Add(item);
            cart.TotalAmount += item.Quantity * item.Price;
            await _context.SaveChangesAsync();
            return Ok(cart);

        }
        [HttpPut("update/{id}")]
        public async Task<IActionResult> UpdateData(int id, [FromBody] CartItem item)
        {
            if (item.Quantity <= 0)
            {
                return BadRequest();
            }
            var cart = await _context.Carts.FindAsync(id);
            var cartitem = await _context.CartItems.FindAsync(item.ItemID);

            cart.TotalAmount -= cartitem.Quantity * cartitem.Price;
            cartitem.Quantity = item.Quantity;
            cart.TotalAmount += item.Quantity * cartitem.Price;
            await _context.SaveChangesAsync();
            return Ok(cart);
        }
        [HttpDelete("delete/{cartId}/{itemId}")]
        public async Task<IActionResult> Deletedata(int cartId,int itemId)
        {
            var cart = await _context.Carts.FindAsync(cartId);
            var citem = await _context.CartItems.FindAsync(itemId);
            if (citem == null)
            {
                return NotFound();
            }
            cart.CartItems.Remove(citem);
            await _context.SaveChangesAsync();
            return Ok(cart);
        }
        [HttpDelete("clear/{cartId}")]
        public async Task<IActionResult> ClearData(int cartId)
        {
            var cart = await _context.Carts.FindAsync(cartId);
            cart.CartItems.Clear();
            cart.TotalAmount = 0;
            await _context.SaveChangesAsync();
            return Ok(cart);
        }
    }
}