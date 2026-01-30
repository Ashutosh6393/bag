using System;

using System.Collections.Generic;

using System.Threading.Tasks;

using CalendarWebApi.DataAccess;

using CalendarWebApi.Models;

using Microsoft.AspNetCore.Mvc;

using Microsoft.Extensions.Logging;

using CalendarWebApi.DTO;

using Mapster;

using Microsoft.EntityFrameworkCore;

using System.Runtime.InteropServices.Marshalling;
 
namespace CalendarWebApi.Controllers

{

    [ApiController]

    [Route("api/calendar")]

    public class CalendarController : ControllerBase

    {

        private readonly ILogger<CalendarController> _logger;

        private readonly IRepository _repo;

        private readonly CalendarContext _context;

 
        public CalendarController(ILogger<CalendarController> logger, IRepository repo, CalendarContext context)

        {

            _logger = logger;

            _repo = repo;

            _context = context;

        }

        [HttpPost]

        public async Task<IActionResult> post(Calendar f)

        {

            var data = await _repo.AddEvent(f);

            await _context.SaveChangesAsync();

            return Created("",data);
 
        }

        [HttpGet]

        public async Task<IActionResult> get()

        {

            var data = await _context.Calendar.ToListAsync();

            //var data = _repo.GetCalendar();

            if (data == null) return NoContent();
 
            return Ok(data);
 
        }

        [HttpGet("query")]

        public async Task<IActionResult> query([FromQuery] EventQueryModel e)

        {

            var data = await _repo.GetCalendar(e);

            if (data == null) return NoContent();

            return Ok(data);

        }

        [HttpPut("{id}")]

        public async Task<IActionResult> updateevent(int id)

        {

            var data = await _context.Calendar.FirstOrDefaultAsync(l => l.Id == id);

            if (data == null) return NotFound();

            await _repo.UpdateEvent(data);

            await _context.SaveChangesAsync();

            return NoContent();

        }
 
        [HttpGet("sort")]

        public async Task<IActionResult> sort()

        {

            var data = await _repo.GetEventsSorted();

            if (data == null) return NoContent();

            return Ok(data);

        }
 
        [HttpDelete("{id}")]

        public async Task<IActionResult> Delete(int id)

        {

            var data = await _context.Calendar.FirstOrDefaultAsync(p => p.Id == id);

            if (data == null) return NotFound();

            _context.Calendar.Remove(data);

            await _context.SaveChangesAsync();

            return NoContent();
 
 
        }
 
    }

}

 