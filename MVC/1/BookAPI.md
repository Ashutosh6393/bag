using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using LibraryService.WebAPI.Data;
using LibraryService.WebAPI.Services;
using LibraryService.WebAPI.DTO;
using Microsoft.EntityFrameworkCore;
 
 
namespace LibraryService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/libraries/{libraryId}/[controller]")]
    public class BooksController : ControllerBase
    {
        private readonly ILibrariesService _librariesService;
        private readonly IBooksService _booksService;
         private readonly LibraryContext _context;
 
 
        public BooksController(IBooksService booksService, ILibrariesService librariesService,LibraryContext context)
        {
            _context=context;
            _librariesService = librariesService;
            _booksService = booksService;
        }
        [HttpPost]
        public async Task<IActionResult> post (int libraryId , BookForm f)
        {
            var res = await _context.Libraries.FirstOrDefaultAsync(x => x.Id == libraryId);
            if(res == null)
            {
                return NotFound();
            }
            var data = new Book
            {
                Name = f.Name,
                Category = f.Category,
                LibraryId = libraryId,
                Id = f.Id
            };
            _context.Books.Add(data);
            await _context.SaveChangesAsync();
            return Created("", data);
        }
        [HttpGet]
        public async Task<IActionResult> get(int libraryId)
        {
            var res = await _context.Libraries.FirstOrDefaultAsync(x => x.Id == libraryId);
            if (res == null)
            {
                return NotFound();
            }
            var data = await _context.Books.Where(x => x.LibraryId == libraryId).ToListAsync();
            return Ok(data);
        }
    }
}
 
 
 
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using LibraryService.WebAPI.Data;
using LibraryService.WebAPI.Services;
using System;
 
namespace LibraryService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class LibrariesController : ControllerBase
    {
        private readonly ILibrariesService _librariesService;
        private readonly LibraryContext _context;
 
        public LibrariesController(ILibrariesService librariesService,LibraryContext context)
        {
            _context=context;
            _librariesService = librariesService;
        }
 
        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            var libraries = await _librariesService.Get(null);
            return Ok(libraries);
        }
 
        [HttpGet("{libraryId}")]
        public async Task<IActionResult> Get(int libraryId)
        {
            var library = (await _librariesService.Get(new[] { libraryId })).FirstOrDefault();
            if (library == null)
                return NotFound();
            return Ok(library);
        }
 
        [HttpPost]
        public async Task<IActionResult> Add(Library l)
        {
            await _librariesService.Add(l);
            return Ok(l);
        }
 
        [HttpPut("{libraryId}")]
        public async Task<IActionResult> Update(int libraryId, Library library)
        {
            var existingLibrary = (await _librariesService.Get(new[] { libraryId })).FirstOrDefault();
            if (existingLibrary == null)
                return NotFound();
 
            await _librariesService.Update(library);
            return NoContent();
        }
 
        // Implement the DELETE method below
        [HttpDelete("{libraryId}")]
        public async Task<IActionResult> DeleteData(int libraryId)
        {
            var lib = await _context.Libraries.FindAsync(libraryId);
            if(lib==null)
            {
                return NotFound();
            }
            _context.Libraries.Remove(lib);
            await _context.SaveChangesAsync();
            return NoContent();
        }
    }
}

 
 