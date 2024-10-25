---
description: This page contains multiple options for grid implementation
---

# Grid

## Grid Methods

### CreateSlickGrid

```typescript
protected createSlickGrid(): Slick.Grid {
	var grid = super.createSlickGrid();
	/*
	* Options
	*/
	return grid;
}
```

### GetSlickOptions

```typescript
protected getSlickOptions(): Slick.GridOptions {
	var opt = super.getSlickOptions();
	/*
	* Options
	*/
	return opt;
}
```

### CreateToolbarExtensions

```typescript
protected createToolbarExtensions() {
	super.createToolbarExtensions();
	/*
	* Options
	*/
}
```

### GetButtons

```typescript
protected getButtons() {
    var buttons = super.getButtons();
    /*
	* Options
	*/
}
```

### GetColumns

```typescript
protected getColumns() {
    var columns = super.getColumns();
    /*
	* Options
	*/
}
```

### GetQuickFilters - Assign Filter choice

In xyz.cshtml page with link like...

```markup
<a href="~/Services/ReportServicesOrderCurrentStatus?status=Shipped" class="small-box-footer">More info <i class="fa fa-arrow-circle-right"></i></a>
```

'status' variable is sent to the grid and assigned to the filter like...

```typescript
protected getQuickFilters(): Serenity.QuickFilter<Serenity.Widget<any>, any>[] {

    let filters = super.getQuickFilters();
    var q = Q.parseQueryString();

    Q.first(filters, x => x.field == fld.Status).init = w => {
        (w as Serenity.StringEditor).value = q["status"];
    };

    return filters;
}
```

### OnViewSubmit

Override ListRequest with custom query

```typescript
protected onViewSubmit(): boolean {
    if (!super.onViewSubmit()) {
        return false;
    }

    let request = this.view.params as Serenity.ListRequest;
    var q = Q.parseQueryString();
    if (q["quote"]) {
        request.Criteria = Serenity.Criteria.and(request.Criteria, [[fld.Id], '=', q["quote"]]);
    }
    if (q["status"]) {
        request.Criteria = Serenity.Criteria.and(request.Criteria, [[fld.Status], '=', q["status"]]);
    }

    return true;

}
```

### Paging options

Enable or Disable pager options

```typescript
protected usePager() {
    return false;
}
```

### GetItemCssClass

Using this method you can create custom conditioning for the cells in SlickGrid.

```typescript
protected getItemCssClass(item: XYZ.XYZ_Row, index: number): string {
    let klass: string = "";
    
    if (item.ClearToBuild == 0)
        klass += " css_class";

    return Q.trimToNull(klass);
}
```

## Grid Options

{% hint style="warning" %}
**REMEMBER!** When adding any new functionality be sure to add:

* Any css files to the project and bundle it in the CssBundles.json file
* Any js files to the project and bundle it in the ScriptBundles.json file
{% endhint %}

### AutoSizeColumns

{% hint style="info" %}
Include _slick.autocolumnsize.js_  in SlickGrid Plugins **>> Rebuild**
{% endhint %}

To enable AutoSizeColumns add...

```typescript
// Columns auto resize
let autoSize = new (Slick as any).AutoColumnSize(true);
grid.registerPlugin(autoSize);

```

to the CreateSlickGrid method

### Cell Selection

**Serenity.CoreLib.d.ts**&#x20;

```typescript
class CellSelectionModel {
}
```

{% hint style="warning" %}
**REBUILD PROJECT!!!**
{% endhint %}

**xyzGrid.ts**

Add to GetSlickOptions method...

```typescript
opt.enableTextSelectionOnCells = true;
opt.selectedCellCssClass = "slickgrid-row-selected";
opt.enableCellNavigation = true;
```

Add to CreateSlickGrid method...

```typescript
grid.setSelectionModel(new Slick.CellSelectionModel());
```

### Row Selection

**xyzGrid.ts**

Add to GetSlickOptions method...

```typescript
opt.enableTextSelectionOnCells = true;
opt.selectedCellCssClass = "slickgrid-row-selected";
opt.enableCellNavigation = true;
```

Add to CreateSlickGrid method...

```typescript
grid.setSelectionModel(new Slick.RowSelectionModel());
```

### Copy Selection

{% hint style="info" %}
Include _slick.cellexternalcopymanager.js_  in SlickGrid Plugins **>> Rebuild**
{% endhint %}

{% hint style="warning" %}
Prerequisites: CellSelection
{% endhint %}

**Serenity.CoreLib.d.ts**&#x20;

Add...

```typescript
class CellExternalCopyManager {
	constructor(options: Slick.CellExternalCopyOptions);
}
interface CellExternalCopyOptions {
	clipboardCommandHandler?: (editCommand: any) => void;
	readOnlyMode?: boolean;
	includeHeaderWhenCopying?: boolean;
	newRowCreator?: (count: any) => void;
}
```

{% hint style="warning" %}
**REBUILD PROJECT!!!**
{% endhint %}

**xyzGrid.ts**

Add to CreateSlickGrid method...

```typescript
var newRowIds = 0;

var pluginOptions = {
	readOnlyMode: false,
	includeHeaderWhenCopying: false,
	newRowCreator: function (count) {
		for (var i = 0; i < count; i++) {
			var item = {
				id: "newRow_" + newRowIds++
			}
			grid.getData().push(item);
		}
	}
};

grid.setSelectionModel(new Slick.CellSelectionModel());
grid.registerPlugin(new Slick.CellExternalCopyManager(pluginOptions));
```

### Default Sort By

```typescript
protected getDefaultSortBy() {
    return [xyz_Row.Fields.Id];
}

protected getDefaultSortBy() {
    return [xyz_Row.Fields.Id + " asc"];
}

protected getDefaultSortBy() {
    return [xyz_Row.Fields.Id + " desc"];
}
```

### Default Grouping

In 'constructor(container: JQuery)' add...

```typescript
this.view.setGrouping(
[{
    getter: 'example'
}])
```

### Overide PersistanceStorage

When using getQuickFilter option while parsing the URL(queryString) Persistance will take precedence over getQuickFilter method. To override this, at the top of the Grid class below the constructor method add...

```typescript
public persistOverride = false;
```

In the getQuickFilter Method add (this.persistOverride = true) to the if statement

```typescript
protected getQuickFilters(): Serenity.QuickFilter<Serenity.Widget<any>, any>[] {

let filters = super.getQuickFilters();
var q = Q.parseQueryString();

if (q["dueStatus"]) {
    this.persistOverride = true//******HERE******
    Q.first(filters, x => x.field == fld.DueStatus).init = w => {
        (w as Serenity.StringEditor).value = q["dueStatus"];
    };
}
```

In the getPersistanceStorage method add an if-else statement like below. Remember to set the boolean back to false after the initial check is true.&#x20;

```typescript
protected getPersistanceStorage(): Serenity.SettingStorage {
    if (this.persistOverride == true) {
        this.persistOverride = false;
        return null;
    } else {
        return new Common.UserPreferenceStorage();
    }
}
```
