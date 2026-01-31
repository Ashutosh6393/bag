//Middleware
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;

namespace LibraryService.WebAPI
{
    public class Middleware
    {

        private readonly RequestDelegate _next;
        static int c;
        public Middleware(RequestDelegate next)
        {
            c = 0;
            _next = next; 
        }
        public async Task InvokeAsync(HttpContext context)
        {
            c++;
            context.Response.OnStarting(() =>
            {
                context.Response.Headers["requestCounter"] = c.ToString();
                return Task.CompletedTask;
            }
            );
            await _next(context);
        }

    }
}
