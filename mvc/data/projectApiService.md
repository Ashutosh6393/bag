userController---------------------------

using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using ProjectService.WebAPI.Services;
using ProjectService.WebAPI.Data;
using ProjectService.WebAPI.DTO;
using Microsoft.EntityFrameworkCore;
namespace ProjectService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/projects/{projectId}/[controller]")]
    public class UsersController : ControllerBase
    {
        private readonly IProjectsService _project;
        private readonly IUsersService _user;
        private readonly ProjectContext _context;

        public UsersController(IProjectsService project,IUsersService user,ProjectContext context)
        {
            _project = project;
            _user = user;
            _context = context;
        }

        [HttpPost]
        public async Task<IActionResult> Add(UserForm U,int projectId)
        {
            var x = await _context.Projects.FirstOrDefaultAsync(x=>x.Id==projectId);
            if (x == null)
            {
                return NotFound();
            }
            var y = new User
            {
                Name = U.Name,
                AddedDate = System.DateTime.Now,   //DateTime wala hai isliye (System.DateTime.Now) kiya hai 
                ProjectId = projectId
            };
            await  _context.Users.AddAsync(y);
            await _context.SaveChangesAsync();
            return Created("",y);
        }

        [HttpGet]
        public async Task<IActionResult> GetData(int projectId)
        {
            var x = await _context.Projects.FirstOrDefaultAsync(x=> x.Id == projectId);
            if (x == null)
            {
                return NotFound();
            }
            var y = await _context.Users.Where(x=> x.ProjectId == projectId).ToListAsync();
            return Ok(y);
        }
    }
}


ProjectController---------------------------


using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using ProjectService.WebAPI.Data;
using ProjectService.WebAPI.Services;

namespace ProjectService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class ProjectsController : ControllerBase
    {
        private readonly IProjectsService _project;
        private readonly IUsersService _user;
        private readonly ProjectContext _context;

        public ProjectsController(IProjectsService project,IUsersService user,ProjectContext context)
        {
            _project =  project;
            _user = user;
            _context = context;
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteProject(int id)
        {
            var x = await _context.Projects.FirstOrDefaultAsync(x=> x.Id == id);
            if( x ==  null)
            {
                return NotFound();
            }
            _context.Projects.Remove(x);
            await _context.SaveChangesAsync();
            return NoContent();
        }
    }
}


DTO UserForm--------------------------------------------
using System;
using Newtonsoft.Json;

namespace ProjectService.WebAPI.DTO
{
    public class UserForm
    {
        [JsonProperty("id")]
        public int Id { get; set; }

        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("projectId")]
        public int ProjectId { get; set; }

        [JsonProperty("addedDate")]
        public DateTime AddedDate { get; set; }
    }
}







