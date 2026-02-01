using System.ComponentModel.DataAnnotations;
 
namespace UserService.WebAPI.Models
{
    public class User
    {
        [Required]
        [MinLength(2,ErrorMessage ="Name is invalid, must contain a minimum of 2 characters")]
        public string Name { get; set; }

 
        [Required]
        [EmailAddress (ErrorMessage ="Email is invalid: Email should be valid and cannot contain example.com as domain")]
        [EmailCheck]
        public string Email { get; set; }
 
        [Required]
        [MinLength(6,ErrorMessage ="Password is invalid: Password must contain 1 small, 1 capital and 1 numeric character")]
        [PasswordCheck]
        public string Password { get; set; }
    }
 
 
     public class EmailCheckAttribute : ValidationAttribute{
 
        protected override ValidationResult IsValid(object? value, ValidationContext validationContext){
 
            string email = value.ToString();
            string[] emailparts = email.Split('@').ToArray();
            if(emailparts.Length !=2){
                return new ValidationResult("Email is invalid: Email should be valid and cannot contain example.com as domain");
            }
             string domain = emailparts[1].ToLower();
             if(domain == "example.com"){
                return new ValidationResult("Email is invalid: Email should be valid and cannot contain example.com as domain");
             }
             return ValidationResult.Success;
        }
    }
 
    public class PasswordCheckAttribute : ValidationAttribute
    {
      protected override ValidationResult IsValid(object? value, ValidationContext validationContext)
      {
         string pass = value.ToString();
         int Upper = 0;
         int Lower = 0;
         int IsDigit = 0;
         foreach(char c in pass){
          if(char.IsUpper(c)){
          Upper++;
         }
         if(char.IsLower(c)){
          Lower++;
         }
         if(char.IsDigit(c)){
          IsDigit++;
         }
         }
         if(!(Upper >= 1 && Lower >= 1  && IsDigit >= 1  ))

 
          {
            return new ValidationResult("Password is invalid: Password must contain 1 small, 1 capital and 1 numeric character");
          }
          return ValidationResult.Success;
      }
    }
}
 
 
 