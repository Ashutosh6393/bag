using System;

using System.ComponentModel.DataAnnotations;
 
namespace CompanyService.WebAPI.Models

{

    public class Company

    {

        [Required]

        [StringLength(35, MinimumLength = 5, ErrorMessage = "CompanyName is invalid: CompanyName must contain a minimum of 5 characters and a maximum of 35, and it must start with 'Company Name:'")]

        [CompanyNameCheck]

        public string CompanyName { get; set; }
 
        [Required]

        [Range(2, int.MaxValue, ErrorMessage = "NumberOfEmployees is invalid: NumberOfEmployees must be greater than 1")]

        public int NumberOfEmployees { get; set; }
 
        [Required]

        [Range(1, int.MaxValue, ErrorMessage = "AverageSalary is invalid: AverageSalary must be greater than 0")]

        public int AverageSalary { get; set; }

    }
 
    public class CompanyNameCheckAttribute : ValidationAttribute

    {

        protected override ValidationResult IsValid(object value, ValidationContext validationContext)

        {

            var name = value.ToString();

            if (string.IsNullOrWhiteSpace(name))

            {

                return new ValidationResult("CompanyName is invalid: CompanyName must contain a minimum of 5 characters and a maximum of 35, and it must start with 'Company Name:'");
 
            }

            if (name.StartsWith("Company Name:"))

            {

                return ValidationResult.Success;

            }

            return new ValidationResult("CompanyName is invalid: CompanyName must contain a minimum of 5 characters and a maximum of 35, and it must start with 'Company Name:'");
 
        }

    }

}
 