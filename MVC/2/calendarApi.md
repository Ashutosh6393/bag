using CalendarWebApi.DataAccess;
using CalendarWebApi.Models;
using Microsoft.AspNetCore.Mvc;
using CalendarWebApi.DTO;
using Microsoft.EntityFrameworkCore;

namespace CalendarWebApi.Controllers
{
    [ApiController]
    [Route("api/calendar")]
    public class CalendarController : ControllerBase
    {
        private readonly ILogger<CalendarController> _logger;
        private readonly CalendarContext _context;

        public CalendarController(ILogger<CalendarController> logger,CalendarContext context)
        {
            _logger = logger;
            _context = context;
        }
        [HttpPost]
        public async Task<IActionResult> add([FromBody] CreateForm f)
        {
            var data = new Calendar
            {
                Name=f.Name,
                Location=f.Location,
                Time=f.Time.Value,
                EventOrganizer=f.EventOrganizer,
                Members=f.Members
            };
            _context.Calendar.Add(data);
            await _context.SaveChangesAsync();
            return StatusCode(201, data);
        }


        [HttpDelete("{id}")]
        public async Task<IActionResult> deletebyif( int id)
        {
            var res = await _context.Calendar.FindAsync(id);
            if(res==null)
            {
                return NotFound();
            }
            _context.Calendar.Remove(res);
            await _context.SaveChangesAsync();
            return NoContent();
        }

        [HttpGet]
        public async Task<IActionResult> getall()
        {
            var res = await _context.Calendar.ToListAsync();
            return Ok(res);
        }

        [HttpPut("{id}")]
        public async Task<IActionResult> update([FromBody] UpdateForm f, [FromRoute] int id)
        {
            if(f==null) return BadRequest();
            var res = await _context.Calendar.FindAsync(id);
            if(res==null) return NotFound();
            res.Name=f.Name;
            res.Time=f.Time.Value;
            res.Location=f.Location;
            res.EventOrganizer=f.EventOrganizer;
            res.Members=f.Members;

            await _context.SaveChangesAsync();
            return Ok();
        }

        [HttpGet("query")]
        public async Task<IActionResult> getbyquery([FromQuery] string? eventOrganizer,int? id,string? location,string? name)
        {
            if(id.HasValue)
            {
                var res = await _context.Calendar.Where(i=>i.Id==id).ToListAsync();
                return Ok(res);
            }
            if(!string.IsNullOrWhiteSpace(eventOrganizer))
            {
                var res = await _context.Calendar.Where(o=>o.EventOrganizer==eventOrganizer).ToListAsync();
                return Ok(res);
            }

            if(!string.IsNullOrWhiteSpace(location))
            {
                var res = await _context.Calendar.Where(o=>o.Location==location).ToListAsync();
                return Ok(res);
            }
            if(!string.IsNullOrWhiteSpace(name))
            {
                var res = await _context.Calendar.Where(o=>o.Name==name).ToListAsync();
                return Ok(res);
            }
            return BadRequest();
        }



        [HttpGet("sort")]
        public async Task<IActionResult> sort()
        {
            var res =_context.Calendar.OrderByDescending(x=>x.Time).ToList();
            return Ok(res);
        }
    }
}
