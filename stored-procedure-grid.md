# Stored Procedure Grid

## Create Request Model

```csharp
using Serenity.Services;
using System;

namespace REACT.LC
{
    public class ReportDeliveryStatusOpenDeliveriesRequest : ListRequest
    {
        public Int32 VariantID { get; set; }
    }

    public class ReportDeliveryStatusOpenDeliveriesResponse : ServiceResponse
    {
        public dynamic[] Values { get; set; }
        //public List<string> Value { get; set; }
    }
}
```

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

## Update Grid

Once you're strong enough, save the world:

```csharp
protected onViewSubmit(): boolean {
    if (!super.onViewSubmit()) {
        return false;
    }

    (this.view.params as ReportDeliveryStatusOpenDeliveriesRequest).VariantID = this.variantId;

    return true;
}
```

### 2 options for calling SP

First Option - EndPoint

```csharp
[HttpPost]
public ListResponse<MyRow> List(IDbConnection connection, ReportDeliveryStatusOpenDeliveriesRequest request)
{
    ListResponse<MyRow> listReturn = new ListResponse<MyRow>();

    var p = new DynamicParameters();
    p.Add("@VariantId", value: request.VariantID, dbType: DbType.Int32, direction: ParameterDirection.Input);
    p.Add("@RecordCount",  dbType: DbType.Int32, direction: ParameterDirection.Output);

    connection.Execute("sp_DeliveryStatusOpenDeliveriesVariant", p, commandType: CommandType.StoredProcedure);

    int p1 = p.Get<int>("@VariantId");
    int p2 = p.Get<int>("@RecordCount");

    listReturn.Entities = (List<MyRow>)connection.Query<MyRow>("sp_DeliveryStatusOpenDeliveriesVariant", p, commandType: CommandType.StoredProcedure);

    //listReturn.Skip = 1;
    listReturn.Take = 100;
    listReturn.TotalCount = p.Get<int>("@RecordCount");

    return listReturn;
}
```

Second Option - Repository

```csharp
public ListResponse<MyRow> List(IDbConnection connection, ReportDeliveryStatusOpenDeliveriesRequest request)
{
    var data = connection.Query<MyRow>("sp_DeliveryStatusOpenDeliveriesVariant",
         param: new
         {
             @variantId = request.VariantID
         },
         commandType: System.Data.CommandType.StoredProcedure);

    var response = new ListResponse<MyRow>();
    response.Entities = (List<MyRow>)data;
    return response;
}
```
