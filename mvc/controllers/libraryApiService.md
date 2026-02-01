bookControler________isme post aue get kiya bsss baki tha

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
        
        private readonly LibraryContext _librarycontext;

        public BooksController(IBooksService booksService, ILibrariesService librariesService,LibraryContext librarycontext)
        {
            _librariesService = librariesService;
            _booksService = booksService;
            _librarycontext = librarycontext;
        }

        [HttpPost]
        public async Task<IActionResult> PostBook(int libraryId,BookForm b) 
        {
            var add = await _librarycontext.Libraries.FirstOrDefaultAsync(x=>x.Id==libraryId);
            if(add == null)
            {
                return NotFound();
            }
            var data = new Book
            {
                Name = b.Name,
                Category= b.Category,
                LibraryId=libraryId     
            };
             await _librarycontext.Books.AddAsync(data);
             await _librarycontext.SaveChangesAsync();
            return Created("",add); 
        }

        [HttpGet]
        public async Task<IActionResult> Getdata(int libraryId)
        {
            var lib = await _librarycontext.Libraries.FirstOrDefaultAsync(x=>x.Id==libraryId);
            if(lib == null)
            {
                return NotFound();
            }
            var inbook = await _librarycontext.Books.Where(x=> x.LibraryId==libraryId).ToListAsync();
            return Ok(inbook);
        }
        // Implement the functionalities below
    }
}


LibraryController_______________________________delete kiya bs baki tha 

using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using LibraryService.WebAPI.Data;
using LibraryService.WebAPI.Services;
using System;
using Microsoft.EntityFrameworkCore;
namespace LibraryService.WebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class LibrariesController : ControllerBase
    {
        private readonly ILibrariesService _librariesService;
        private readonly LibraryContext _libContext;

        public LibrariesController(ILibrariesService librariesService , LibraryContext context) 
        {
            _librariesService = librariesService;
            _libContext = context;
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
        [HttpDelete("{libraryId}")]
        public async Task<IActionResult> Deletelib(int libraryId)
        {
            var dlib = await _libContext.Libraries.FirstOrDefaultAsync(x=>x.Id==libraryId);
            if(dlib==null)
            {
                return NotFound();
            }
            var dbook = await _libContext.Books.Where(x=>x.Id==libraryId).ToListAsync();
            if(dbook.Count > 0)
            {
                 _libContext.Books.RemoveRange(dbook);
            }
            _libContext.Libraries.Remove(dlib);
            await _libContext.SaveChangesAsync();
            return NoContent();
        }
    }
}