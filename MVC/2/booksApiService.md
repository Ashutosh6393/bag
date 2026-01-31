//model
using System;
using System.ComponentModel.DataAnnotations;

namespace BooksService.WebAPI.Models
{
    public class Book
    {
        [Required(ErrorMessage = "Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case")]
        [StringLength(255,MinimumLength =5,ErrorMessage = "Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case")]
        [UpperTitle]
        public string Title { get; set; }
        [Required(ErrorMessage = "Author is invalid: Author must contain a minimum of 3 characters and a maximum of 30, and the first letter should be in upper case")]
        [StringLength(30,MinimumLength=3,ErrorMessage = "Author is invalid: Author must contain a minimum of 3 characters and a maximum of 30, and the first letter should be in upper case")]
        [AuthorUpper]

        public string Author { get; set; }
        [Required(ErrorMessage = "PublicationDate is invalid: PublicationDate must be after 01/01/1900 and before the current date")]
        [Publish]
 
        public DateTime PublicationDate { get; set; }
    }
    public class UpperTitleAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid(object? value,ValidationContext validationContext)
        {
            string s = value.ToString();
            if(string.IsNullOrWhiteSpace(s))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            if(!(char.IsUpper(s[0])))
            {
                return new ValidationResult("Title is invalid: Title must contain a minimum of 5 characters and a maximum of 255, and the first letter should be in upper case");
            }
            return ValidationResult.Success;
        }
    }
    public class AuthorUpperAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid(object? value,ValidationContext validationContext)
        {
            string s = value.ToString();
            if(string.IsNullOrWhiteSpace(s))
            {
                return new ValidationResult("Author is invalid: Author must contain a minimum of 3 characters and a maximum of 30, and the first letter should be in upper case");
            }
            if(!(char.IsUpper(s[0])))
            {
                return new ValidationResult("Author is invalid: Author must contain a minimum of 3 characters and a maximum of 30, and the first letter should be in upper case");
            }
            return ValidationResult.Success;
        }
    }
    public class PublishAttribute:ValidationAttribute
    {
        protected override ValidationResult? IsValid(object? value,ValidationContext validationContext)
        {
            //impformat
            DateTime old = new DateTime(1990,1,1);
            DateTime curr = DateTime.Now;
            if(value==null)
            {
                return new ValidationResult("PublicationDate is invalid: PublicationDate must be after 01/01/1900 and before the current date");
            }
            DateTime val = Convert.ToDateTime(value);
            if(val>old&&val<curr)
            {
                return ValidationResult.Success;
            }
            return new ValidationResult("PublicationDate is invalid: PublicationDate must be after 01/01/1900 and before the current date");
        }
    }
}