# Override List Request Handler - Repository

```csharp
    private class MyListHandler : ListRequestHandler<MyRow>
    {
      protected override void ApplyFilters(SqlQuery query)
      {
        base.ApplyFilters(query);
        query.Where(new Criteria(fld.deleted).In(1);
      }
    }
```

Using SubQuery to prefilter a grid

```csharp
        private class MyListHandler : ListRequestHandler<MyRow> {

            protected override void ApplyFilters(SqlQuery query)
            {
                base.ApplyFilters(query);
                var m = Entities.ModulesRow.Fields;
                query.Where(new Criteria(fld.ModuleId).In(query.SubQuery()
                                        .From(m)
                                        .Select(m.Id)
                                        .Where(new Criteria(m.Enabled).In(1))));
            }
        }
```
