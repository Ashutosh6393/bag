________Document API Services (validation)_________

using Microsoft.AspNetCore.Http.HttpResults
using System;
using System.ComponentModel.DataAnnotations;
using System.Globalization;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
namespace DocumentService.WebAPI.Models
{
    public class Document
    {
        [Required]
        [TtileCheck]
        [StringLength(35, MinimumLength = 5, ErrorMessage = "Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter")]

        public string Title { get; set; }
        [Required]
        [Range(1, 499, ErrorMessage = "Size is invalid: Size must be greater than 0 MB and less than 500 MB")]

        public int Size { get; set; }
        [Required]
        [FormatCheck]
        public string Format { get; set; }

    }

    public class TtileCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
        {
            string title = value.ToString();
            string[] words = title.Split(' ').ToArray();
            if (string.IsNullOrWhiteSpace(title))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter");
            }
            foreach (var word in words)
            {
                if (!char.IsUpper(word[0]))
                {
                   return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 35, and each word should start with an uppercase letter");
                }
            }
            return ValidationResult.Success;
        }
    }
    public class FormatCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
        {
           string format = value.ToString();
            if (string.IsNullOrWhiteSpace(format))
            {
                return new ValidationResult("Format is invalid: Format must be lowercase and equal one of the following: 'txt', 'pdf', 'docx'");
            }
            if (format == "pdf" || format == "docx" || format == "txt" && format == format.ToLower())
            {
                return ValidationResult.Success;
            }
            return new ValidationResult("Format is invalid: Format must be lowercase and equal one of the following: 'txt', 'pdf', 'docx'");
        }
    }
}

