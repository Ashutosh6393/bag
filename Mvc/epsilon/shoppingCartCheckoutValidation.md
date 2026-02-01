//CheckoutController
using Microsoft.AspNetCore.Mvc;
using ShoppingCart.Checkout.WebAPI.Data;
using ShoppingCart.Checkout.WebAPI.Dto;
using ShoppingCart.Checkout.WebAPI.Models;
using ShoppingCart.Checkout.WebAPI.SeedData;
using ShoppingCart.Checkout.WebAPI.Services;

namespace ShoppingCart.Checkout.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]

    public class CheckoutController : ControllerBase
    {
        private readonly ICheckoutService checkoutService;
        private readonly CheckoutContext _context;
        public CheckoutController(ICheckoutService service, CheckoutContext context)
        {
            _context=context;

            checkoutService = service;
        }
        [HttpPost("validate")]
        public async Task<IActionResult> AddCust([FromBody] OrderDTO dto)
        {
            if(dto.Products==null && dto.Customer == null)
            {
                return BadRequest();
            }
            int status = 1;
            List<string> errors = new List<string>();

            string[] narr = dto.Customer.Name?.Split(' ',StringSplitOptions.RemoveEmptyEntries);
            if(string.IsNullOrWhiteSpace(dto.Customer.Name) || narr.Length <=2)
            {
                status = 0;
                errors.Add("The name must contain more than two words");
            }
            string[] earr = dto.Customer.Email?.Split("@",StringSplitOptions.RemoveEmptyEntries);
            if(earr==null||earr.Length!=2)
            {
                status = 0;
                errors.Add("Invalid customer's email address.");
            }
            if(string.IsNullOrWhiteSpace(dto.Customer.Phone) || dto.Customer.Phone.Length!=10)
            {
                status = 0;
                errors.Add("The customer phone number format is incorrect");
            }
            string[] aarr = dto.Customer.Address?.Split(' ', StringSplitOptions.RemoveEmptyEntries);
            if(string.IsNullOrWhiteSpace(dto.Customer.Address)|| aarr.Length<=2)
            {
                status = 0;
                errors.Add("The address must contain more than two words");
            }

            if(!(dto.Products.Any()))
            {
               status =0;
                errors.Add("Products can't empty");
            }

            foreach(var item in dto.Products)
            {
                var data = await _context.Products.FindAsync(item.ProductID);
                if (data==null)
                {
                    status = 0;
                    errors.Add($"product with ID {item.ProductID} does not exist");
                }
                if(item.Quantity<1)
                {
                    status = 0;
                    errors.Add("Product quantity must be at least 1");
                }
            }
            if(status == 0)
            {
                return BadRequest(
                    new
                    {
                        Status = status,
                        Message = "Order validation errors",
                        Errors= errors
                    });
            }
            return Ok();    
            
        }
        [HttpPost("addOrder")]
        public async Task<IActionResult> AddOrder(OrderDTO dto)
        {
            if (dto.Products == null && dto.Customer == null)
            {
                return BadRequest();
            }
            int status = 1;
            List<string> errors = new List<string>();

            string[] narr = dto.Customer.Name?.Split(' ', StringSplitOptions.RemoveEmptyEntries);
            if (string.IsNullOrWhiteSpace(dto.Customer.Name) || narr.Length <= 2)
            {
                status = 0;
                errors.Add("The name must contain more than two words");
            }
            string[] earr = dto.Customer.Email?.Split("@", StringSplitOptions.RemoveEmptyEntries);
            if (earr == null || earr.Length != 2)
            {
                status = 0;
                errors.Add("Invalid customer's email address.");
            }
            if (string.IsNullOrWhiteSpace(dto.Customer.Phone) || dto.Customer.Phone.Length != 10)
            {
                status = 0;
                errors.Add("The customer phone number format is incorrect");
            }
            string[] aarr = dto.Customer.Address?.Split(' ', StringSplitOptions.RemoveEmptyEntries);
            if (string.IsNullOrWhiteSpace(dto.Customer.Address) || aarr.Length <= 2)
            {
                status = 0;
                errors.Add("The address must contain more than two words");
            }

            if (!(dto.Products.Any()))
            {
                status = 0;
                errors.Add("Products can't empty");
            }

            foreach (var item in dto.Products)
            {
                var data = await _context.Products.FindAsync(item.ProductID);
                if (data == null)
                {
                    status = 0;
                    errors.Add($"product with ID {item.ProductID} does not exist");
                }
                if (item.Quantity < 1)
                {
                    status = 0;
                    errors.Add("Product quantity must be at least 1");
                }
            }
            if (status == 0)
            {
                return BadRequest(
                    new
                    {
                        Status = status,
                        Message = "Order validation errors",
                        Errors = errors
                    });
            }

            var cdata = new Customer
            {
                Phone = dto.Customer.Phone,
                Address = dto.Customer.Address,
                Name = dto.Customer.Name,
                Email = dto.Customer.Email
            };

            List<OrderProduct> orders = new List<OrderProduct>();
            
            double total = 0;
            foreach (var item in dto.Products) {
                var pro = await _context.Products.FindAsync(item.ProductID);
                total += item.Quantity * pro.Price;

                var no = new OrderProduct
                {
                    Product = pro,
                    ProductID = item.ProductID,
                    Quantity = item.Quantity,
                };
                orders.Add(no);
            }

            var dataa = new Order
            {
                Customer = cdata,
                Total = total,
                OrderProducts = orders
            };
            _context.Orders.Add(dataa);
            await _context.SaveChangesAsync();
            return Ok(
                new
                {
                    Status = status,
                    Data = dataa
                }
                );
        }


    }
}
