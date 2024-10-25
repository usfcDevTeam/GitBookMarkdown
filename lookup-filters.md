---
description: This page will explain how to use Lookup Filters
---

# Lookup Filters

{% hint style="danger" %}
Before any new lookup will be added you must \[Rebuild Solution] and then \[Transform All T4 Templates]!!!
{% endhint %}

![LookupEditor based in Group table based off of ModuleName from the Module table](<.gitbook/assets/image (24).png>)

{% hint style="warning" %}
&#x20;First make sure you have a `[LookupScript]`attribute on all tables you want to lookup from
{% endhint %}

### LookupInclude

Add `LookupInclude` attribute to xyz\_Row. We need it to be available at client side, otherwise they are not included by default in lookup scripts.\


```csharp
[DisplayName("Id"), Column("ID"), Identity, LookupInclude]
[FormCssClass("line-break-lg")]
public Int32? Id
{
	get { return Fields.Id[this]; }
	set { Fields.Id[this] = value; }
}
```

\[LookupInclude] is usually used with ID fields but can also be used with other fields.

```csharp
[DisplayName("Enabled"), NotNull, LookupInclude]
public Boolean? Enabled
{
	get { return Fields.Enabled[this]; }
	set { Fields.Enabled[this] = value; }
}
```

### LookupEditor

Add \[LookupEditor] to the foreign key field tied to the \[LookupInclude] field in the other table

```csharp
[DisplayName("Module Id"), Column("ModuleID"), NotNull, ForeignKey("[dbo].[Modules]", "ID"), LeftJoin("jModules"), TextualField("Module")]
[LookupEditor(typeof(General.Entities.ModulesRow))]
[FormCssClass("line-break-lg")]
public Int32? ModuleId
{
	get { return Fields.ModuleId[this]; }
	set { Fields.ModuleId[this] = value; }
}
```

To filter the \[LookupEditor] simply add \[FilterField] and \[FilterValue]

```csharp
[DisplayName("Module Id"), Column("ModuleID"), NotNull, ForeignKey("[dbo].[Modules]", "ID"), LeftJoin("jModules"), TextualField("Module")]
[LookupEditor(typeof(General.Entities.ModulesRow), FilterField = nameof(ModulesRow.Fields.Enabled), FilterValue = true)]
[FormCssClass("line-break-lg")]
public Int32? ModuleId
{
	get { return Fields.ModuleId[this]; }
	set { Fields.ModuleId[this] = value; }
}
```

### Custom Lookups

For custom lookups a select statement is created. This gives the ability to query what will be listed in the combobox/dropdown.

```csharp
    [LookupScript]
    public class xyz_AreaLookup : RowLookupScript<xyz_Row>
    {
        public AgendaAreaLookup()
        {
            IdField = "AreaId";
            TextField = "AreaChoiceName";
        }

        protected override void PrepareQuery(SqlQuery query)
        {
            var fld = xyz_Row.Fields;
            query.Distinct(true)
                .Select(fld.AreaId)
                .Select(fld.AreaChoiceName)
                .Where(
                    new Criteria(fld.Id) != "" &
                    new Criteria(fld.Id).IsNotNull());
        }

        protected override void ApplyOrder(SqlQuery query)
        {
        }
    }
```

To apply the Lookup, add it to xyz\_Columns

```csharp
        [Width(125), DisplayName("Area"), LookupEditor(typeof(Lookups.AgendaAreaLookup)), QuickFilter(CssClass = "hidden-xs")]
        public String AreaChoiceName { get; set; } 
```

### Lookup Query

```csharp
var odbot = LC.ReportOpenDeliveriesByOrderTypeRow.getLookup().items;
var dl = odbot.filter(x => x.OrderType == orderType).map(x => ({ Mg1: x.Mg1, Dl: x.Dl }));
```
