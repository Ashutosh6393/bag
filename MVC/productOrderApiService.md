//order Controller
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using ProductOrderApi.Data;
using ProductOrderApi.Data.Entities;
using ProductOrderApi.Data.Models;
using ProductOrderApi.Services;
using System.Numerics;
using Microsoft.EntityFrameworkCore;

namespace ProductOrderApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class OrdersController : ControllerBase
    {
        private readonly OrderService _orderService;
        private readonly OrderContext _context;
        public OrdersController(OrderService orderService, OrderContext context)
        {
            _context=context;
            _orderService = orderService;
        }
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Order>>> GetOrders()
        {
          var order = await _context.Orders.ToListAsync();
            return Ok(order);
        }
        [HttpGet("{id}")]
        public async Task<ActionResult<Order>> GetOrder(int id)
        {
            var order = await _context.Orders.FindAsync(id);
            if (order == null)
            {
                return NotFound();
            }
            return Ok(order);

        }
        [HttpPost]
        public async Task<ActionResult<Order>> AddOrder([FromBody]CreateOrderModel model)
        {
            decimal total = 0;
            var data = model.OrderProducts;
            foreach(var item in data)
            {
                var pro = await _context.Products.FindAsync(item.ProductId);
                total += pro.Price * item.Quantity;
            }
            var o = new Order
            {
                TotalPrice = total,
                OrderDate = DateTime.Now
            };
            _context.Orders.Add(o);
            await _context.SaveChangesAsync();
            return Ok(o);   

        }
        [HttpPut("{id}")]
        public async Task<IActionResult> UpdateOrder(int id, Order order)
        {
            if (id != order.Id)
            {
                return BadRequest();
            }
            var o = await _context.Orders.FindAsync(id);
            if (o == null) 
            {
                return NotFound();
            }
            o.TotalPrice= order.TotalPrice;
            o.OrderProducts = order.OrderProducts;
            o.OrderDate= DateTime.Now;
            
            await _context.SaveChangesAsync();
            return NoContent();

        }
        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteOrder(int id)
        {
            var order = await _context.Orders.FindAsync(id);
            if(order == null)
            {
                return NotFound();
            }
            _context.Orders.Remove(order);
            await _context.SaveChangesAsync();
            return NoContent();

        }

        
    }
}
//Product Controller
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using ProductOrderApi.Data;
using ProductOrderApi.Data.Entities;
using ProductOrderApi.Data.Repositories;
using ProductOrderApi.Helpers;
using ProductOrderApi.Services;
using SQLitePCL;
using Microsoft.EntityFrameworkCore;
using System.Runtime.InteropServices;

namespace ProductOrderApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
        private readonly ProductService _productService;
        private readonly ProductRepository _productRepository;
        private readonly OrderContext _context;


        public ProductsController(ProductService productService,OrderContext context)
        {
            _context = context;
            _productService = productService;
        }
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
        {
            var pro = await _context.Products.ToListAsync();
            return Ok(pro); 


        }
        [HttpGet("{id}")]
        public async Task<ActionResult<Product>> GetProduct(int id)
        {
            var gpro= await _context.Products.FindAsync(id);
            if(gpro == null)
            { return NotFound();
            }
            return Ok(gpro);  

        }
        [HttpPost]
        public async Task<ActionResult<Product>> AddProduct(Product product)
        {
           _context.Products.Add(product);  
            await _context.SaveChangesAsync();
            return Ok(product);

        }
        [HttpPut("{id}")]
        public async Task<IActionResult> UpdateProduct(int id,[FromBody] Product product)
        {
            if(id!=product.Id)
            {
                return BadRequest();
            }
            var pro = await _context.Products.FindAsync(id);
            if (pro == null)
            {
                return NotFound();
            }
            pro.Price = product.Price;
            pro.Name = product.Name;
            await _context.SaveChangesAsync();
            return NoContent();

        }
        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteProduct(int id)
        {
            var pro = await _context.Products.FindAsync(id);  
            if(pro==null)
            {
                return NotFound();  
            }
            _context.Products.Remove(pro);
            await _context.SaveChangesAsync();
            return NoContent();


        }
    }
}
