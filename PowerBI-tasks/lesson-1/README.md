<h1>Project: Sales Performance Dashboard</h1>

<ul>
  <li>Task1: Download Power BI Desktop and connect to a sample Excel dataset</li>
  <li>Task2: Explore the Power BI interface and describe the key sections(e.g report view, data view, and model view)</li>
</ul>

<hr>
<p>I downloaded Power BI Desktop and installed on my computer. I created new Workspace and loaded dataset(salseperformce.xlsx) to Power BI Desktop.</p>
<h2>M code in Advanced Editor</h2>
<code>
let
    Source = Excel.Workbook(File.Contents("C:\Users\hpvic\OneDrive\Documents\MAAB\PowerBI\lesson-1\dataset\Sales_Performance.xlsx"), null, true),
    Sheet1_Sheet = Source{[Item="Sheet1",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Sheet1_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type date}, {"Product Name", type text}, {"Sales Amount", Int64.Type}, {"Salesperson", type text}, {"Region", type text}, {"Units Sold", Int64.Type}, {"Column7", type any}}),
    #"Removed Duplicates" = Table.Distinct(#"Changed Type", {"Date"}),
    #"Split Column by Delimiter" = Table.SplitColumn(Table.TransformColumnTypes(#"Removed Duplicates", {{"Date", type text}}, "en-US"), "Date", Splitter.SplitTextByDelimiter("/", QuoteStyle.Csv), {"Date.1", "Date.2", "Date.3"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Date.1", Int64.Type}, {"Date.2", Int64.Type}, {"Date.3", Int64.Type}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type1",{{"Date.2", "Date"}, {"Date.1", "Month"}, {"Date.3", "Year"}}),
    #"Reordered Columns" = Table.ReorderColumns(#"Renamed Columns",{"Date", "Month", "Year", "Product Name", "Sales Amount", "Salesperson", "Region", "Units Sold", "Column7"}),
    #"Added Month Name" = Table.AddColumn(#"Reordered Columns", "Month Name", each Date.MonthName(#date([Year], [Month], 1)), type text),
    #"Reordered Columns1" = Table.ReorderColumns(#"Added Month Name",{"Date", "Month", "Month Name", "Year", "Product Name", "Sales Amount", "Salesperson", "Region", "Units Sold", "Column7"}),
    #"Removed Columns" = Table.RemoveColumns(#"Reordered Columns1",{"Date", "Month", "Column7"})
in
    #"Removed Columns"
</code>
