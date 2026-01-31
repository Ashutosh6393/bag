//Controller
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Caching.Memory;
using NewsFeedService.WebAPI.Data;
using NewsFeedService.WebAPI.Services;

namespace NewsFeedService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class NewsFeedController : ControllerBase
    {
        private readonly INewsFeedService _newsFeedService;
        private readonly IMemoryCache _memoryCache;
        const string Key = "abc";

        public NewsFeedController(INewsFeedService newsFeedService, IMemoryCache memoryCache)
        {
            _newsFeedService = newsFeedService;
            _memoryCache = memoryCache;
        }

        [HttpGet("{id}")]
        public async Task<IActionResult> Get(int id)
        {
            var user = (await _newsFeedService.Get(new[] { id }, null)).FirstOrDefault();
            if (user == null)
                return NotFound();

            return Ok(user);
        }

        [HttpGet]
        public async Task<IActionResult> GetAll([FromQuery] Filters filters)
        {
            if(!(_memoryCache.TryGetValue(Key,out List<NewsFeedItem> _cachedItem)))
            {
                var newsItems = await _newsFeedService.Get(null, filters);
                _memoryCache.Set(
                    Key,
                    newsItems,
                    new MemoryCacheEntryOptions()
                    {
                        AbsoluteExpirationRelativeToNow = TimeSpan.FromSeconds(2)
                    }
                    );
                return Ok(newsItems);
            }
            

            return Ok(_cachedItem);
        }

        [HttpPost]
        public async Task<IActionResult> Add(NewsFeedItem newsFeedItem)
        {
            await _newsFeedService.Add(newsFeedItem);
            _memoryCache.Remove(Key);
            return Ok(newsFeedItem);
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> Delete(int id)
        {
            var user = (await _newsFeedService.Get(new[] { id }, null)).FirstOrDefault();
            if (user == null)
                return NotFound();

            await _newsFeedService.Delete(user);
            _memoryCache.Remove(Key);
            return NoContent();
        }
    }
}
