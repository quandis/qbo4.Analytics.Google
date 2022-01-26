# qbo4.Analytics.Google Requirements

Use Google Sheets to drive calculations for an API endpoint

Implement the following psuedo-code as a class library in .netstandard2.1, leveraging the [Google Sheets API](https://developers.google.com/sheets/api/quickstart/dotnet) with cooresponding `xUnit` tests:

``` csharp
public Task<JObject> MergeAsync(JObject data, Uri googleSpreadsheetUrl) 
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

Tests:

```csharp
[Theory]
[InlineData(100000, 208.33)]
[InlineData(150000, 312.50)]
public async Task Calculates(double balance, double payment) 
{
  var url = "https://docs.google.com/spreadsheets/d/1f0J7bvcAjj21wK32CqfVJsr_jLHJiL-jIm9_Y9kXGiI";
  var input = new JObject();
  input["GrossUPB"] = balance;
  input["PrincipalInterest"] = 1500;
  input["Taxes"] = 250;
  
  var result = await MergeAsync(input, url);
  Assert.Equal(payment, double.Parse(result["Payment"]));
}
```

A [sample spreadsheet](https://docs.google.com/spreadsheets/d/1f0J7bvcAjj21wK32CqfVJsr_jLHJiL-jIm9_Y9kXGiI/edit?usp=sharing) includes:

- Comments in cells B5..B7
- Names ranges for cells E21..E22

