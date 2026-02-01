//Middleware---in---PasswordCheckerMiddleware.cs
using System.Net;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;
 
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
            if (!context.Request.Headers.TryGetValue("passwordKey", out var value))
            {
                context.Response.StatusCode = StatusCodes.Status403Forbidden;
                return;
            }
            if (value != "passwordKey123456789")
            {
                context.Response.StatusCode = StatusCodes.Status403Forbidden;
                return;
            }
            // Call the next middleware in the pipeline
 
            await _next(context);
        }
    }
}
 
 