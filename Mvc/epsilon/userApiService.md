--userValidation----
--Model

using System.ComponentModel.DataAnnotations;

namespace UserService.WebAPI.Models
{
    public class User
    {
        [Required]
        [MinLength(2,ErrorMessage = "Name is invalid, must contain a minimum of 2 characters")]
        public string Name { get; set; }

        [Required]
        [EmailAddress]
        [EmailCheck]
        public string Email { get; set; }

        [Required]
        [MinLength(6,ErrorMessage ="Password is invalid: Password must contain 1 small, 1 capital and 1 numeric character")]
        [PasswordCheck]
        public string Password { get; set; }
    }
}

-----------------------------------------------------------------------------------------------------------------------------
--PasswordCheck------CustomValidation

using System.ComponentModel.DataAnnotations;

namespace UserService.WebAPI.Models
{
    public class PasswordCheckAttribute : ValidationAttribute
    {
      protected override ValidationResult IsValid(object? value, ValidationContext validationContext)
      {
          string Password = value.ToString();
          int  upper = 0;
          int  lower = 0;
          int  digit = 0;

          foreach(char c in Password)
          {
            if(char.IsUpper(c))
            {
              upper++;
            }
            if(char.IsLower(c))
            {
              lower++;
            }
            if(char.IsDigit(c))
            {
              digit++;
            }
          }
          if(!(upper >= 1 && lower >= 1  && digit >= 1  ))
          {
            return new ValidationResult("Password is invalid: Password must contain 1 small, 1 capital and 1 numeric character");
          }
          return ValidationResult.Success;
      }
    }
}

-----------------------------------------------------------------------------------------------------------------------------
--EmailCheck------CustomValidation


using System.ComponentModel.DataAnnotations;

namespace UserService.WebAPI.Models
{
    public class EmailCheckAttribute : ValidationAttribute
    {
        protected override ValidationResult? IsValid (object? value , ValidationContext validationContext)
        {
            string Email = value.ToString();
            string[] emailparts = Email.Split('@');
            if(emailparts.Length != 2)
            {
                return new ValidationResult("Email is invalid: Email should be valid and cannot contain example.com as domain");
            }
            if(emailparts[1]== "example.com")
            {
                return new ValidationResult("Email is invalid: Email should be valid and cannot contain example.com as domain");
            }
            return ValidationResult.Success;
        }
    }
}

----------------------------------------------------------------------------------------------------------------------------------