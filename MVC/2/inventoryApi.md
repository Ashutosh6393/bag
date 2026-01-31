Inventary API On calling on controller--------------------------------------------------------------------------------

using InventoryWebApi.DataAccess;
using Microsoft.AspNetCore.Mvc;
using InventoryWebApi.Models;
using Microsoft.EntityFrameworkCore;

namespace InventoryWebApi.Controllers
{
    [ApiController]
    [Route("inventory/item")]
    public class InventoryController : ControllerBase
    {
        private readonly IRepository _repository;
        private readonly InventoryContext _context;

        public InventoryController(IRepository repo,InventoryContext context)
        {
            _context=context;
            _repository=repo;
        }

        [HttpPost]
        public async Task<IActionResult> AddItem(InventoryItem inventoryItem)
        {
            var x = await _repository.AddInventoryItem(inventoryItem);
            return Created("",x);
        }

        [HttpDelete("{Barcode}")]
        public async Task<IActionResult> DeleteItem(int Barcode)
        {
            var y = await _context.Inventory.FirstOrDefaultAsync(x=> x.Barcode == Barcode);
            if(y == null)
            {
                return Ok();
            }
            await _repository.DeleteInventoryItem(y); 
            return NoContent();
        }

        [HttpGet("query")]
        public async Task<IActionResult> GetFromQuery([FromQuery] ItemQueryModel query)
        {
            var x  = _repository.GetInventoryItem(query);
            return Ok(x);
        }

        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            var items = await _repository.GetInventory();
            return Ok(items);
        }

        [HttpGet("sort")]
        public async Task<IActionResult> SortedData()
        {
            var x = await _repository.GetSorted();
            return Ok(x);
        }

        [HttpPut("{barcode}")]
        public async Task<IActionResult> UpdateData(InventoryItem inventoryItem)
        {
            var x =  await _repository.UpdateInventoryItem(inventoryItem);
            if( x == null)
            {
                return NotFound();
            }
            return NoContent();
        }
    }
}

-----------------------------------------------------------------------------------------------------------------------------------

Complete code on controller--------------------------------------------------------------------------------

using InventoryWebApi.DataAccess;
using Microsoft.AspNetCore.Mvc;
using InventoryWebApi.Models;
using InventoryWebApi.DTO;
using Microsoft.EntityFrameworkCore;

namespace InventoryWebApi.Controllers
{
    [ApiController]
    [Route("inventory/item")]
    public class InventoryController : ControllerBase
    {
        private readonly IRepository _repository;
        private readonly InventoryContext _context;

        public InventoryController(IRepository repo,InventoryContext context)
        {
            _context=context;
            _repository=repo;
        }

        [HttpPost]
        public async Task<IActionResult> AddData(CreateForm c)
        {
            var x = new InventoryItem
            {
                Name = c.Name,
                Category = c.Category,
                Price = c.Price,
                Quantity = c.Quantity,
                Discount = c.Discount
            };
            await _context.Inventory.AddAsync(x);
            await _context.SaveChangesAsync();
            return Created("",x);
        }

        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            var x = await _context.Inventory.ToListAsync();
            return Ok(x);
        }

       [HttpGet("query")]
       public async Task<IActionResult> GetByQuery([FromQuery]ItemQueryModel q)
       {
         IQueryable<InventoryItem> item = _context.Inventory;
         if( q.Barcode > 0)
         {
            await item.Where(x=> x.Barcode == q.Barcode).ToListAsync();
            return Ok(item);
         }
         if(!string.IsNullOrWhiteSpace(q.Name))
         {
            await item.Where(x=> x.Name == q.Name).ToListAsync();
            return Ok(item);
         }
         if(!string.IsNullOrWhiteSpace(q.Category))
         {
            await item.Where(x=> x.Category == q.Category).ToListAsync();
            return Ok(item);
         }
         if( q.Discount > 0)
         {
            await item.Where(x=> x.Discount == q.Discount).ToListAsync();
            return Ok(item);
         }
          return Ok();
       }

        [HttpGet("sort")]
        public async Task<IActionResult> SortData()
        {
            var x = await  _context.Inventory.OrderByDescending(o=> o.Price).ToListAsync();
            return Ok(x);
        }

        [HttpDelete("{barcode}")]
        public async Task<IActionResult> DeleteData(int barcode)
        {
            var x = await  _context.Inventory.FirstOrDefaultAsync(x=> x.Barcode == barcode);
            if (x == null)
            {
                return NotFound();
            }
            return NoContent();
        }

        [HttpPut("{barcode}")]
        public async Task<IActionResult> UpdateData(int barcode , Query q)
        {
           var x = await  _context.Inventory.FirstOrDefaultAsync(x=> x.Barcode == barcode);
            if (x == null)
            {
                return NotFound();
            } 
             x.Barcode = barcode;
             x.Category = q.category;
             x.Discount = q.discount;
             x.Name = q.name;
              _context.Inventory.Update(x);
             await _context.SaveChangesAsync();
             return NoContent();      
        }
    }
}


-------------------------------------------------------------------------------------------------------




