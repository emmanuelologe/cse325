# Module Assignment Notes

## Part 1: ASP.NET Core Web API

I completed the Create a web API with ASP.NET Core controllers module.

The original Pizzas List contained:

- Classic Italian
- Veggie

I added the following additional record:

- Pepperoni

### API Testing Evidence

#### GET
Request:
GET /pizza

Status Code:
200 OK

#### POST
Request:
POST /pizza

Status Code:
201 Created

#### PUT
Request:
PUT /pizza/3

Status Code:
204 No Content

#### DELETE
Request:
DELETE /pizza/3

Status Code:
204 No Content

---

## Part 2: Sales Summary

I added a function called `GenerateSalesSummary`.

The function:

- Reads the sales files from the sales directory.
- Calculates the total sales for each file.
- Calculates the overall sales total.
- Uses a `StringBuilder` to create the report.
- Writes the final report to `salesSummary.txt`.

### Sales Summary Function

```csharp
static void GenerateSalesSummary(string salesFolder)
{
    string[] salesFiles = Directory.GetFiles(salesFolder);

    decimal totalSales = 0;

    StringBuilder report = new StringBuilder();

    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");

    List<string> details = new List<string>();

    foreach (string file in salesFiles)
    {
        decimal fileTotal = 0;

        string[] sales = File.ReadAllLines(file);

        foreach (string sale in sales)
        {
            fileTotal += decimal.Parse(sale);
        }

        totalSales += fileTotal;

        string fileName = Path.GetFileName(file);

        details.Add($"  {fileName}: {fileTotal:C}");
    }

    report.AppendLine($" Total Sales: {totalSales:C}");
    report.AppendLine();
    report.AppendLine(" Details:");

    foreach (string detail in details)
    {
        report.AppendLine(detail);
    }

    File.WriteAllText(
        Path.Combine(salesFolder, "salesSummary.txt"),
        report.ToString()
    );
}