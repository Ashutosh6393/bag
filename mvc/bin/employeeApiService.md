------------------Employee API Service --------------
------------------controller-------------------------




using Microsoft.AspNetCore.Mvc;
using EmployeeCollection.WebAPI.Data;
using EmployeeCollection.WebAPI.Services;
using Microsoft.EntityFrameworkCore;
using EmployeeCollection.WebAPI.SeedData;
 
namespace EmployeeCollection.WebAPI.Controllers
{
   [ApiController]
    [Route("api/[controller]")]
  public class EmployeeController : ControllerBase
  {
    private readonly IEmployeeCollectionService _service;
    private readonly EmployeeCollectionContext _context;
    public EmployeeController(IEmployeeCollectionService service, EmployeeCollectionContext context)
    {
      _service = service;
      _context = context;
    }
 
    [HttpPost]
    public async Task<IActionResult> AddEmployee(CreateEmployeeForm e)
    {
        var x = new Employee
        {
          Name = e.Name,
          Location = e.Location,
          Age = e.Age,
          Rating = e.Rating,
          Department = e.Department
        };
        await _context.Employees.AddAsync(x);
        await _context.SaveChangesAsync();
         return Created("",x);
    }


    [HttpGet]
    public async Task<IActionResult> GetByQuerry([FromQuery] Filters f)
    {
      var x =await _service.GetEmployees(null,f);
      return Ok(x);
    }

 
    [HttpGet("{id}")]
    public async Task<IActionResult> GetById(int id)
    {
      var x = await _context.Employees.FirstOrDefaultAsync(x=> x.Id==id);
      if( x == null)
      {
        return NotFound();
      }
      return Ok(x);
    }

 
    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteById(int id)
    {
      var x = await _context.Employees.FirstOrDefaultAsync(x=> x.Id==id);
      if( x == null)
      {
        return NotFound();
      }
      //await _service.DeleteEmployee(x);
      _context.Employees.Remove(x);
      await _context.SaveChangesAsync();
      return NoContent();
    }
  }
}