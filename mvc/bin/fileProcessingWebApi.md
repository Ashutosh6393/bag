//FileController
using System;

using System.Collections.Generic;

using System.IO;

using System.Linq;

using System.Text;

using System.Threading.Tasks;

using System.Xml;

using System.Xml.Linq;

using Mapster;

using Microsoft.AspNetCore.Http;

using Microsoft.AspNetCore.Mvc;

using FileProcessing.WebAPI.Models;

using FileProcessing.WebAPI.SeedData;

namespace FileProcessing.WebAPI.Controllers

{

    [ApiController]

    [Route("api/file")]

    public class FileController : ControllerBase

    {

        [HttpPost("analyze")]

        public IActionResult AnalyzeFile([FromBody] byte[] fileContent)

        {

            var xml = Encoding.UTF8.GetString(fileContent);

            var doc = XDocument.Parse(xml);

            var rates = doc.Descendants("rate")

                           .Select(x => int.Parse(x.Value))

                           .ToList();

            var result = new StatisticalModel

            {

                MinRate = rates.Min(),

                MaxRate = rates.Max(),

                AverageRate = (float)rates.Average()

            };

            return Ok(result);

            //return Ok();

        }

        [HttpPost("adduser")]

        public IActionResult AddUsersToFile([FromBody] AddUsersToFileModelForm model)

        {

            //// encoded text

            string base64 = model.Content;

            // original file bytes

            byte[] bytes = Convert.FromBase64String(base64);

            // readable XML

            string xml = Encoding.UTF8.GetString(bytes);

            var doc = XDocument.Parse(xml);

            var root = doc.Element("UserCollection");

            foreach (var user in model.Users)

            {

                root.Add(

                    new XElement("User",

                    new XElement("id", user.Id),

                           new XElement("first_name", user.FirstName),

                           new XElement("last_name", user.LastName),

                           new XElement("email", user.Email),

                           new XElement("rate", user.Rate)

                                        ));

            }

            return Ok(doc.ToString());


        }

    }

}
 
//TestMiddleware
namespace FileProcessing.WebAPI.Models

{

    public class TokenMiddleware

    {

        private readonly RequestDelegate _next;

        public TokenMiddleware(RequestDelegate next)

        {

            _next = next;

        }

        public async Task Invoke(HttpContext context)

        {

            var path = context.Request.Path.Value?.ToLower();

            // /api/testmiddleware/token

            if (path != null && path.Contains("/api/testmiddleware/token"))

            {

                var token = context.Request.Query["token"].FirstOrDefault();

                if (token != "12345678")

                {

                    context.Response.StatusCode = StatusCodes.Status403Forbidden;

                    return;

                }

                context.Response.StatusCode = StatusCodes.Status200OK;

                return;

            }

            // /api/testmiddleware/notoken

            if (path != null && path.Contains("/api/testmiddleware/notoken"))

            {

                context.Response.StatusCode = StatusCodes.Status200OK;

                return;

            }

            await _next(context);

        }

    }

}
 