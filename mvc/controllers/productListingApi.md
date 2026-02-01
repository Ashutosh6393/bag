using Microsoft.AspNetCore.Mvc;
using Product.WebAPI.Data;
using Product.WebAPI.Helpers;
using Product.WebAPI.SeedData;
using Product.WebAPI.Services;
using Microsoft.EntityFrameworkCore;
 
namespace Product.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
 
    public class ProductController : ControllerBase
    {
        private readonly ProductContext _context;
        public ProductController(ProductContext context)
        {
            {
                _context = context;
            }
        }
        [HttpPost]
        public async Task<IActionResult> AddData([FromBody]CreateProductForm form)
        {
            var data = new ProductItem
            {
                Active = form.Active,
                Brand = form.Brand,
                Description = form.Description,
                ImageLink = form.ImageLink,
                Name = form.Name,
                Price = form.Price,
            };
            _context.ProductItems.Add(data);
            await _context.SaveChangesAsync();
            return Created("", data);
        }
        [HttpGet("{id}")]
        public async Task<IActionResult> GetByID(int id)
        {
            var data = await _context.ProductItems.FindAsync(id);
            if(data==null)
            {
                return NotFound();
            }
            return Ok(data);
        }
        [HttpGet]
        public async Task<IActionResult> GetByQuery([FromQuery] Filters f)
        {
            var query = _context.ProductItems.Where(x => x.Active).AsQueryable();
            if(!string.IsNullOrWhiteSpace(f.Name))
            {
                query = query.Where(x=>x.Name.Contains(f.Name));
            }
            if(f.PriceFrom.HasValue && f.PriceTo.HasValue)
            {
                query = query.Where(x=>x.Price>=f.PriceFrom.Value && x.Price<=f.PriceTo);
            }
            if(!string.IsNullOrWhiteSpace(f.Brand))
            {
                query = query.Where(x => x.Brand.Contains(f.Brand));
            }
            if (!string.IsNullOrWhiteSpace(f.Sort))
            {
                if (f.Sort.Equals("name", StringComparison.OrdinalIgnoreCase)) 
                {
                    query = f.SortDir.Equals("asc",StringComparison.OrdinalIgnoreCase)?query.OrderBy(x=>x.Name):query.OrderByDescending(x=>x.Name);
                }
                else
                {
                    query = f.SortDir.Equals("asc", StringComparison.OrdinalIgnoreCase) ? query.OrderBy(x => x.Price) : query.OrderByDescending(x => x.Price);
                }
            }
            var data = await query.ToListAsync();
            return Ok(
                new
                {
                    TotalProducts= data.Count,
                    Products = data,
                }
                );
        }
    }
}

 
 