using System;
using System.ComponentModel.DataAnnotations;
namespace BooksService.WebAPI.Models
{
    public class Book
    {
        [Required]
        [StringLength(255,MinimumLength = 5, ErrorMessage = "Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case")]
        [TitleCheck]
        public string Title { get; set; }
        [Required]
        [StringLength(30, MinimumLength = 3, ErrorMessage = "Author is invalid: Author must contain a minimum of 3 characters and a maximum of 30, and the first letter should be in upper case")]
        [Author]
        public string Author { get; set; }
        [Required]
        [Date]
        public DateTime PublicationDate { get; set; }
    }
    public class TitleCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult IsValid(object value, ValidationContext validationContext)
        {
            string title = value.ToString();
            if (string.IsNullOrEmpty(title))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            if (!char.IsUpper(title[0]))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            return ValidationResult.Success;
        }
    }
    public class AuthorAttribute : ValidationAttribute
    {
        protected override ValidationResult IsValid(object value, ValidationContext validationContext)
        {
            string author = value.ToString();
            if (string.IsNullOrEmpty(author))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            if (!char.IsUpper(author[0]))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            return ValidationResult.Success;
        }
    }
    public class DateAttribute : ValidationAttribute
    {
        protected override ValidationResult IsValid(object value, ValidationContext validationContext)
        {
            DateTime date = (DateTime)value;
            if (date >= new DateTime(1900, 01, 01) && date <= DateTime.Today)
            {
                return ValidationResult.Success;
            }
            return new ValidationResult("PublicationDate is invalid: PublicationDate must be after 01/01/1900 and before the current date");
        }
    }
}
 
 