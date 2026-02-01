Document API Services 

---------------Model-------------------------
using System;
using System.ComponentModel.DataAnnotations;
using System.Globalization;
using System.Linq;

namespace DocumentService.WebAPI.Models
{
    public class Document
    {
        [Required]
        [StringLength(35,MinimumLength = 5 , ErrorMessage = "Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter")]
        [TitleCheck]
        public string Title { get; set; }

        [Required]
        [Range(1,499,ErrorMessage ="Size is invalid: Size must be greater than 0 MB and less than 500 MB")]
        public int Size { get; set; }

        [Required]
        [FormatCheck]
        public string Format { get; set; }
    }
}

__________________________________________________________________________________________________________________________

------------------------------------FormatCheck-------------------------------------------

using System;
using System.ComponentModel.DataAnnotations;
using System.Globalization;
using System.Linq;

namespace DocumentService.WebAPI.Models
{
    public class FormatCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid (object? value , ValidationContext validationContext)
        {
           string Format = value.ToString();
           if(string.IsNullOrWhiteSpace(Format))
           {
            return new ValidationResult("Format is invalid: Format must be lowercase and equal one of the following: 'txt', 'pdf', 'docx'");
           }
           if(!(Format=="txt" || Format=="pdf"|| Format=="docx"))
           {
               return new ValidationResult("Format is invalid: Format must be lowercase and equal one of the following: 'txt', 'pdf', 'docx'");
           }
           return ValidationResult.Success;
        }
    }
}

__________________________________________________________________________________________________________________________

------------------------------------TitleCheck-------------------------------------------

using System;
using System.ComponentModel.DataAnnotations;
using System.Globalization;
using System.Linq;

namespace DocumentService.WebAPI.Models
{
    public class TitleCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid (object? value , ValidationContext validationContext)
        {
           string Title = value.ToString();
           if(string.IsNullOrWhiteSpace(Title))
           {
            return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter");
           }
           string[] t = Title.Split(' ').ToArray();
           foreach(string c in t)
           {
             if(!(char.IsUpper(c[0]))) 
             {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter");
             }
           }
           return ValidationResult.Success;
        }
    }
}

____________________________________________________________________________________________________________________________________________________