//Middleware
using System.Net;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Http.HttpResults;
using System.Net;

namespace RoomService.WebAPI
{
   
    public class PasswordCheckerMiddleware
    {
        private readonly RequestDelegate _next;
        public PasswordCheckerMiddleware(RequestDelegate next)
        {
            _next = next; 
        }
        public async Task InvokeAsync(HttpContext context)
        {
            if (context.Request.Headers["passwordKey"]!="passwordKey123456789")
            {
                context.Response.StatusCode = (int)HttpStatusCode.Forbidden; return;
            }
            await _next(context);
        }
    }
}
