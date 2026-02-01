---------------Company API Service------------------------
----Model

using System;
using System.ComponentModel.DataAnnotations;

namespace CompanyService.WebAPI.Models
{
    public class Company
    {
        [Required]
        [StringLength(35,MinimumLength =(5),ErrorMessage ="CompanyName is invalid: CompanyName must contain a minimum of 5 characters and a maximum of 35, and it must start with 'Company Name:'")]
        [CompanyNameCheck]
        public string CompanyName { get; set; }

        [Required]
        [NumberOfEmployeesCheck]
        public int NumberOfEmployees { get; set; }

        [Required]
        [AverageSalaryCheck]
        public int AverageSalary { get; set; }
    }
}

--------------------------------------------------------------------------------------
----------AverageSalaryCheck---------------------

using System;
using System.ComponentModel.DataAnnotations;

namespace CompanyService.WebAPI.Models
{
    public class AverageSalaryCheckAttribute : ValidationAttribute
    {
      protected override ValidationResult? IsValid(Object? value , ValidationContext validationContext)
      {
         int AverageSalary = (int) value;
          if(!(AverageSalary > 0))
          {
            return new ValidationResult("AverageSalary is invalid: AverageSalary must be greater than 0");
          }        
        return ValidationResult.Success;
      }
    }
}


--------------------------------------------------------------------------------------
----------NumberOfEmployeesCheck---------------------


using System;
using System.ComponentModel.DataAnnotations;

namespace CompanyService.WebAPI.Models
{
    public class NumberOfEmployeesCheckAttribute : ValidationAttribute
    {
      protected override ValidationResult? IsValid(Object? value , ValidationContext validationContext)
      {
         int NumberOfEmployees = (int) value;
          if(!(NumberOfEmployees > 1))
          {
            return new ValidationResult("NumberOfEmployees is invalid: NumberOfEmployees must be greater than 1");
          }        
        return ValidationResult.Success;
      }
    }
}


--------------------------------------------------------------------------------------
----------CompanyNameCheck---------------------


using System;
using System.ComponentModel.DataAnnotations;

namespace CompanyService.WebAPI.Models
{
    public class CompanyNameCheckAttribute : ValidationAttribute
    {
      protected override ValidationResult? IsValid(Object? value , ValidationContext validationContext)
      {
         string Name = value.ToString();
         if(!(Name.StartsWith("Company Name:")))
         {
            return new ValidationResult("CompanyName is invalid: CompanyName must contain a minimum of 5 characters and a maximum of 35, and it must start with 'Company Name:'");
         }
         return ValidationResult.Success;
      }
    }
}
