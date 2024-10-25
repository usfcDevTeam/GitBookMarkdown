# Row Options

## Field Parameters

Field Parameters are above each field declaration. The parameters are in-cased in \[ and ].&#x20;

### Expression

An expression allows for the manipulation of the query or select statement. (example: concat two fields)&#x20;

```typescript
[Expression("(t0.FName + ' ' + t0.LName)")]
```

### DisplayName

Use DisplayName to change the name of the field to be inherited by Form and Column classes

```typescript
[DisplayName("Display Name")]
```



### Quick Search

Adding Quick Search allows that field to be included in the default search box that is at the top of each grid.

```typescript
[QuickSearch]
```

### Unique Constraint

{% embed url="https://github.com/volkanceylan/Serenity/issues/4092" %}

### IsActive

Adding the IsActive feature to your Row class will give the ability to toggle a datarow on or off.&#x20;

#### Step 1 - Add an IsActive Column to your Database Table.

![In order for IsActive to work it must be of datatype int](<.gitbook/assets/image (27).png>)

#### Step 2 - Add the IsActive Column to your xyzRow

```csharp
public class RowFields : RowFieldsBase
{
    public Int16Field IsActive;
}
```

```csharp
[DisplayName("IsActive"), NotNull, Insertable(false), Updatable(true), DefaultValue(1), BooleanEditor, LookupInclude]
public Int16? IsActive
{
    get { return Fields.IsActive[this]; }
    set { Fields.IsActive[this] = value; }
}
```

#### Step 3 - Add IIsActiveRow to your xyzRow

```csharp
public sealed class ProjectsRow : Row, IIdRow, INameRow, IIsActiveRow
```

```csharp
Int16Field IIsActiveRow.IsActiveField
{
    get { return Fields.IsActive; }
}
```

{% hint style="warning" %}
Build Project and Transform all T4 Templates
{% endhint %}

#### Step 4 - Add IsActive implementation to xyzGrid and xyzDialog

```csharp
protected getIsActiveProperty() { return ProjectsRow.isActiveProperty; }
```

![](<.gitbook/assets/image (17).png>)

![](<.gitbook/assets/image (29).png>)

#### Step 5 - Add IsActive field to the xyzColumns

```csharp
public Int16 IsActive { get; set; }
```

### IsActiveDeleted

IsActiveDeletedRecord goes hand in hand with the IsActive feature. This feature allows you to soft Deleted a record and also gives you the ability to undelete the record.

{% hint style="info" %}
You must have the IsActive feature implemented
{% endhint %}

#### Step 1 - Add IsActiveDeletedRow to xyzRow

```csharp
public sealed class ProjectsRow : Row, IIdRow, INameRow, IIsActiveDeletedRow
```

#### Step 2 - Add Undelete to xyzRepository

```csharp
private class MyUndeleteHandler : UndeleteRequestHandler<MyRow> { }
```

```csharp
public UndeleteResponse Undelete(IUnitOfWork uow, UndeleteRequest request)
{
    return new MyUndeleteHandler().Process(uow, request);
}
```

#### Step 3 - Add Undelete to xyzEndpoint

```csharp
[HttpPost, AuthorizeDelete(typeof(MyRow))]
public UndeleteResponse Undelete(IUnitOfWork uow, UndeleteRequest request)
{
    return new MyRepository().Undelete(uow, request);
}
```
