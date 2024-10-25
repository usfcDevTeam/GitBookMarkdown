# Columns

### Update Columns

```typescript
protected updateColumns(columnlookup: string[]) {
   let columns = this.getColumns();
    let newColumns = [];
    columnlookup.forEach((value) => {
        if (columns.some(f => f.field === value))
            newColumns.push(columns.filter(f => f.field === value).shift());
    });

    this.slickGrid.setColumns(newColumns);
}
```

#### Column format

```typescript
this.updateColumns(['OtmShipment', 'Delivery', 'MaterialNumber'])
```
