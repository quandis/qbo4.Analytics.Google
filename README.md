# qbo4.Analytics.Google
Use Google Sheets to drive calculations for an API endpoint

Implement the following method in .netstandard2.1 with cooresponding tests:

``` csharp
public JObject Merge(JObject data, Uri googleSpreadsheetUrl) 
{
  var results = new JObject();
  using (var spreadsheet = Clone(googleSpreadsheetUrl))
  {
    foreach (comment in spreadsheet)
      if (comment is a JsonPath expression)
        set the comment's associated cell value to the JsonPath value
      
    foreach (namedRange in spreadsheet)
      if (namedRange comprises just 1 cell)
        add to the results
  }
  return results;  
}

private Clone(url) 
{
  // ensure multiple calls do not overwrite each other by using the same spreadsheet at the same time
}
```

