Movie service Localization
 
public LocalizedString this[string name]
{
     get
     {
         var culture = CultureInfo.CurrentUICulture.TwoLetterISOLanguageName;
         if (!resources.ContainsKey(culture))
         {
             culture = "en";
         }
         string data=string.Empty;
         if (resources[culture].ContainsKey(name))
         {
             data = resources[culture][name];
         }
         else
         {
             data = name;
         }
 
         return new LocalizedString(name,data);
     }
}