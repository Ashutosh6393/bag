using System;

using System.Collections.Generic;

using System.Threading.Tasks;

using CalendarWebApi.DataAccess;

using CalendarWebApi.Models;

using Microsoft.AspNetCore.Mvc;

using Microsoft.Extensions.Logging;

using CalendarWebApi.DTO;

using Mapster;

using CalendarWebApi.DataAccess;

using Microsoft.EntityFrameworkCore;
 
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

        public async Task<IActionResult> AddCal(CreateForm form)

        {

            var cal = new Calendar

            {

                Name = form.Name,

                Time = form.Time ?? 0,

                Location = form.Location,

                EventOrganizer = form.EventOrganizer,

                Members = form.Members

            };

            await _context.Calendar.AddRangeAsync(cal);

            await _context.SaveChangesAsync();

            return Created("", cal);

        }
 
        [HttpGet]

        public async Task<IActionResult> GetAll()

        {

            var cal = await _context.Calendar.ToListAsync();

            return Ok(cal);

        }
 
        [HttpGet("query")]

        public async Task<IActionResult> GetByQuery([FromQuery] EventQueryModel query)

        {

            var cal = await _repo.GetCalendar(query);

            return Ok(cal);

        }
 
        [HttpGet("sort")]

        public async Task<IActionResult> GetSort()

        {

            var cal = await _repo.GetEventsSorted();

            return Ok(cal);

        }
 
        [HttpPut("{id}")]

        public async Task<IActionResult> Update(int id, Calendar calendarEvent)

        {

            var cal = await _context.Calendar.FirstOrDefaultAsync(x => x.Id == id);

            if (cal == null)

            {

                return NotFound();

            }
 
            await _repo.UpdateEvent(calendarEvent);

            return NoContent();

        }
 
        [HttpDelete("{id}")]

        public async Task<IActionResult> Delete(int id)

        {

            var cal = await _context.Calendar.FirstOrDefaultAsync(x => x.Id == id);

            if (cal == null)

            {

                return NotFound();

            }

            _context.Calendar.Remove(cal);

            await _context.SaveChangesAsync();

            return NoContent();

        }

    }

}
 