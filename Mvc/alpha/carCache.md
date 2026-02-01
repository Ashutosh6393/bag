//controller
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using CarService.WebAPI.Data;
using CarService.WebAPI.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Caching.Memory;

namespace CarService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class CarsController : ControllerBase
    {
        private readonly ICarsService _carsService;
        private readonly IMemoryCache _cache;
        const string Key = "abc";
        

        public CarsController(ICarsService carsService, IMemoryCache cache)
        {
            _carsService = carsService;
            _cache = cache;
        }

        [HttpGet("{id}")]
        public async Task<IActionResult> Get(int id)
        {
            var user = (await _carsService.Get(new[] { id }, null)).FirstOrDefault();
            if (user == null)
                return NotFound();

            return Ok(user);
        }

        [HttpGet]
        public async Task<IActionResult> GetAll([FromQuery] Filters filters)
        {
            if(!(_cache.TryGetValue(Key,out List<Car> _CachedItem)))
            {
                var cars = await _carsService.Get(null, filters);
                _cache.Set(
                    Key,
                    cars,
                    new MemoryCacheEntryOptions
                    {
                        AbsoluteExpirationRelativeToNow = TimeSpan.FromSeconds(2)
                    });
                return Ok(cars);    
            }
           

            return Ok(_CachedItem);
        }

        [HttpPost]
        public async Task<IActionResult> Add(Car car)
        {
            await _carsService.Add(car);
            _cache.Remove(Key);
            return Ok(car);
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> Delete(int id)
        {
            var user = (await _carsService.Get(new[] { id }, null)).FirstOrDefault();
            if (user == null)
                return NotFound();

            await _carsService.Delete(user);
            _cache.Remove(Key);
            return NoContent();
        }
    }
}
