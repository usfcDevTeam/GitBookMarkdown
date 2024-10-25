---
description: This page will describe how to add buttons to the toolbar of a grid
---

# Adding Buttons

{% hint style="info" %}
Replace 'xyz\_' with your module name
{% endhint %}

### Initialize Buttons

#### In xyz\_Grid.ts add:

```typescript
        protected getButtons() {
            var buttons = super.getButtons();
            //...........//
            return buttons;
        }
```

### Excel Buttons

#### Export:

&#x20;[>>Implementation Youtube Video Found Here<<](https://youtu.be/qk1CRralnQQ)

Adding an excel export to your table is simpler than you think. ReportHelper and ExcelExportHelper do most of the work for you when setting things up. You just need to add code in 2 places: xyzGrid.ts and xyzEndpoint.cs:

&#x20;In xyzGrid.ts add

```typescript
protected getButtons() {
        var buttons = super.getButtons();

        buttons.push(myProject.Common.ExcelExportHelper.createToolButton({
            grid: this,
            onViewSubmit: () => this.onViewSubmit(),
            service: xyz_Service.baseUrl + '/ListExcel',
            separator: true,
            hint: "This is the Hint",
            title:"This is the Title"
        }));

        return buttons;
}
```

Explanation of fields:

* Grid: takes the current view of the grid (if you hid columns using column picker it ignores them)
* onViewSubmit: action being taken when event occurs
* service: Location of list repository (See xyzEndpoint.cs)
* hint: Caption that appears in the button itself.
* title: Caption that appears as label/name of button when you hover



In order to make the Excel export button work update xyz\_Endpoint.cs file, adding the following code

```csharp
using Serenity.Reporting;
using Serenity.Web;
using System;

//.....///

public FileContentResult ListExcel(IDbConnection connection, ListRequest request)
    {
        var data = List(connection, request).Entities;
        var report = new DynamicDataReport(data, request.IncludeColumns, typeof(Columns.xyz_Columns));
        var bytes = new ReportRepository().Render(report);
        var reportName = "xyz";
        return ExcelContentResult.Create(bytes, reportName + "_" + DateTime.Now.ToString("yyyyMMdd_HHmmss") + ".xlsx");
    }
```

#### Import:

Coming soon....

## File Download Buttons

In the getButtons Sub add

```typescript
            buttons.push(Common.FileDownloadHelper.createToolButton({
                title: 'Template',
                grid: this,
                service: xyz_Service.baseUrl + '/DownloadFile',
                onViewSubmit: () => this.onViewSubmit(),
                separator: true
            }));
```

Add a FileResult Sub in xyz\_Endpoint.cs

```csharp
        public FileResult DownloadFile()
        {
            string path = AppDomain.CurrentDomain.BaseDirectory + "Documents/";
            byte[] fileBytes = System.IO.File.ReadAllBytes(path + "Template.xlsx");
            string fileName = "Template.xlsx";
            return File(fileBytes, System.Net.Mime.MediaTypeNames.Application.Octet, fileName);
        }
```

{% hint style="warning" %}
Be sure to Rebuild solution and then Transform all T4 Templates
{% endhint %}

### PDF Buttons

Start by going to the xyzGrid.ts file of the table you wish to add this to. Then add the following code to add the PDF button with minimum edits. Just adding these few lines already enables PDF export. :

```typescript
//....//
 getButtons() {
        var buttons = super.getButtons();
     
        buttons.push(myProject.Common.PdfExportHelper.createToolButton({
            grid: this,
            onViewSubmit: () => this.onViewSubmit()
        }));

        return buttons;
    }
```

If you want to try using some more advanced custom features then:

```typescript
//....//
getButtons() {
        var buttons = super.getButtons();

        buttons.push(myProject.Common.PdfExportHelper.createToolButton({
            grid: this,
            onViewSubmit: () => this.onViewSubmit(),
            title: "This is the Title",
            hint: "This is the hint",
            separator: true,
            reportTitle: "This is report title",
            titleTop: 2,
            titleFontSize: 12,
            fileName: "ThisIsTheFileName",
            pageNumbers: true,
            columnTitles: {
                'OriginalColumn': 'OCol.',
            },               
            tableOptions: {
                 columnStyles: {
                    OriginalColumn: {
                        columnWidth: 25,
                        halign: 'right'
                    }
                }
            }
        }));

        return buttons;
    }
```



Explanation of fields:

* Grid: takes the current view of the grid (if you hid columns using column picker it ignores them)
* onViewSubmit: action being taken when event occurs
* hint: Caption that appears in the button itself.
* title: Caption that appears as label/name of button when you hover
* separator: Adds a separation line between this button and other buttons on the toolbar.
* reportTitle: The Report Title will appear as the name of the report on the first PDF page and will also be the default name of the file when being downloaded.
* titleTop: How many lines from the top should the title have as a buffer.
* titleFontSize: Controls the font size of the reportTitle field when added to the PDF view,
* fileName: \[Unsure]
* pageNumbers: if "true" then page numbers will be added at the end of each page. Example: 1/10
* columnTitles: Allows you to manipulate how the column titles appear on the PDF \*\*See Northwind Product Grid for implementation example
* tableOptions: Allows you to manipulate the visual effects of the table Example: Width \*\*See Northwind Product Grid for implementation example
