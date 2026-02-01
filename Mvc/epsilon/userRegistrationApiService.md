-------UserRegisteration------
_______controller_____________

using dotnet_registration_api.Data;
using dotnet_registration_api.Data.Entities;
using dotnet_registration_api.Data.Models;
using dotnet_registration_api.Data.Repositories;
using dotnet_registration_api.Helpers;
using dotnet_registration_api.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace dotnet_registration_api.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class UsersController : ControllerBase
    {
        private readonly UserService _userService;
        private readonly UserDataContext _context;
        private readonly UserRepository _repo;
        public UsersController(UserService userService, UserDataContext context, UserRepository repo)
        {
            _userService = userService;
            _context = context; 
            _repo = repo;
        }
        [HttpPost("login")]
        public async Task<ActionResult<User>> Login([FromBody]LoginRequest model)
        {
            var check = await _context.Users.FirstOrDefaultAsync(x=> x.Username == model.Username);
            if (check == null)
            {
                return BadRequest();
            }
            if(check.PasswordHash != HashHelper.HashPassword(model.Password))
            {
                return BadRequest();
            }
            return Ok();
        }
        [HttpPost("register")]
        public async Task<ActionResult<User>> Register([FromBody]RegisterRequest model)
        {
            var check = await _context.Users.FirstOrDefaultAsync(x => x.Username == model.Username);
            if (check != null)
            {
                return BadRequest();
            }
            var newuser = new User
            {
                Username = model.Username,
                FirstName = model.Username,
                LastName = model.Username,
                PasswordHash = HashHelper.HashPassword(model.Password)
            };
            await _context.Users.AddAsync(newuser);
            await _context.SaveChangesAsync();
            return Ok(newuser);
        }
        [HttpGet]
        public async Task<ActionResult<IEnumerable<User>>> GetAll()
        {
            var getAll = await _context.Users.ToListAsync();
            return Ok(getAll);
        }
        [HttpGet("{id}")]
        public async Task<ActionResult<User>> GetById(int id)
        {
            var getBY = await _context.Users.FirstOrDefaultAsync(x=> x.Id == id);
            if(getBY == null)
            {
                return NotFound();
            }
            return Ok(getBY);
        }
        [HttpPut("{id}")]
        public async Task<ActionResult<User>> Update(int id, [FromBody] UpdateRequest model)
        {
            var user = await _context.Users.FirstOrDefaultAsync(x => x.Id == id);
            if (user == null)
            {
                return NotFound();
            }
            if (user.PasswordHash != HashHelper.HashPassword(model.OldPassword))
            {
                return BadRequest();
            }
            if (user.Username != model.Username)
            {
                var alreadyUser = await _context.Users.FirstOrDefaultAsync(x=> x.Username == model.Username);
                if (alreadyUser != null)
                {
                    return BadRequest();
                }
            }
            user.Username = model.Username;
            user.FirstName = model.FirstName;
            user.LastName = model.LastName;
            user.PasswordHash = HashHelper.HashPassword(model.NewPassword);
            await _context.SaveChangesAsync();
            return Ok();
        }
        [HttpDelete("{id}")]
        public async Task<ActionResult> Delete(int id)
        {
            var deleteData = await _context.Users.FirstOrDefaultAsync(x => x.Id == id);
            if (deleteData == null)
            {
                return NotFound();
            }
            _context.Users.Remove(deleteData);
            await _context.SaveChangesAsync();
            return Ok();
        }
    }
}
