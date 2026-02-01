Complete code in Controller _______________________
using Microsoft.AspNetCore.Mvc;

using RestaurantCollection.WebApi.DataAccess;

using RestaurantCollection.WebApi.DTO.Forms;

using RestaurantCollection.WebApi.Models;

using Microsoft.EntityFrameworkCore;
 
namespace RestaurantCollection.WebApi.Controllers

{

    [ApiController]

    [Route("api/[controller]")]

    public class RestaurantController : ControllerBase

    {

        private readonly IRepository _repository;

        private readonly RestaurantCollectionContext _context;

        public RestaurantController(IRepository repository, RestaurantCollectionContext context)

        {

            _repository = repository;

            _context = context;

        }
 
        [HttpPost]

        public async Task<IActionResult> AddRestro(CreateForm c)

        {

            var x = new Restaurant

            {

                AverageRating = c.Rating,

                Name = c.Name,

                Votes = c.Votes,

                City = c.City

            };

            await _context.Restaurants.AddAsync(x);

            await _context.SaveChangesAsync();

            return Created("", x);

        }
 
        [HttpPut("{id}")]

        public async Task<IActionResult> UpdateRestro(int id, UpdateForm u)

        {

            var x = await _context.Restaurants.FirstOrDefaultAsync(z => z.Id == id);

            if (x == null)

            {

                return NotFound();

            }

            x.AverageRating = u.Rating;

            x.Votes = u.Votes;

            _context.Restaurants.Update(x);

            await _context.SaveChangesAsync();

            return NoContent();

        }
 
        [HttpGet]

        public async Task<IActionResult> GetAll()

        {

            var result = await _context.Restaurants.ToListAsync();

            return Ok(result);

        }
 
        [HttpGet("query")]

        public async Task<IActionResult> GetByQuery([FromQuery] RestaurantQueryModel q)

        {

            IQueryable<Restaurant> query = _context.Restaurants;

            if (!string.IsNullOrWhiteSpace(q.City))

            {

                query = query.Where(x => x.City == q.City);

            }

            if (q.Id > 0)

            {

                query = query.Where(x => x.Id == q.Id);

            }

            var x = await query.ToListAsync();

            return Ok(x);

        }
 
        [HttpDelete("{id}")]

        public async Task<IActionResult> DeleteRestro(int id)

        {

            var x = await _context.Restaurants.FirstOrDefaultAsync(x => x.Id == id);

            if (x == null)

            {

                return NotFound();

            }

            _context.Restaurants.Remove(x);

            await _context.SaveChangesAsync();

            return NoContent();

        }
 
        [HttpGet("sort")]

        public async Task<IActionResult> SortRestro()

        {

            var x = await _context.Restaurants.OrderBy(o => o.AverageRating).ToListAsync();

            return Ok(x);

        }

    }

}

 
Restaurant API Service
 