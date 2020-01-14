# Basic GraphQL in ASP .NET Core (Queries)

สมมติว่าเรามีข้อมูล User ในระบบอยู่แล้ว แล้วเราอยากจะดูว่ามีอะไรบ้าง

```json
query {
  getUser{
    _id
    index
    balance
    favoriteFruit
    age
    eyeColor
    email
  }
}
```

เมื่อยิงคำสั่งนี้ไป เราก็จะได้ข้อมูล JSON ที่มีรายละเอียดตามนี้มา
ต่อไปเราจะเอาแค่บาง fields มาแล้วกัน ก็อยากได้แค่ไอดีงาน, ชื่องาน, ปีที่จัด, สถานที่ที่จัด และข้อมูลของผู้ร่วมงาน 

และนี่คือผลลัพธ์ของเราจาก request ที่เรา customize

```
{
  "data": {
    "getUser": [
      {
        "_id": "5de76602976429f85cf4d605",
        "index": 107,
        "balance": "$2,330.97",
        "favoriteFruit": "banana",
        "age": 30,
        "eyeColor": "green",
        "email": "florence.neal@buzzness.co.uk"
      },
      {
        "_id": "5de766027bcda76c80d7f1f3",
        "index": 108,
        "balance": "$1,403.98",
        "favoriteFruit": "banana",
        "age": 19,
        "eyeColor": "green",
        "email": "saunders.carver@zentix.com"
      }
  }
}
```

ส่วนโค๊ดที่เกี่ยวข้องนั้น มีหลักๆ อยู่ 3 ชุด คือ
-   query
-   Schema Query
-   Schema Types

## Queries

คือการ Querie ข้อมูล ตามที่เราระบุไว้ในฟิลของ query ที่เราส่งไป เช่น
```json
query {             //จะทำการ query
  getUser{          //โดยใช้ฟิลที่ชื่อ getUser โดยมีข้อมุลที่ต้องการดังนี้
    _id             // id
    index           //index
    balance         //balance
    favoriteFruit
    age
    eyeColor
    email
  }
}
```

## Schema

คือระบุการทำงานของข้อมูลที่เราจะใช้

```c#
public class AppSchema : Schema
    {
        public AppSchema(IDependencyResolver resolver)
            :base(resolver)
        {
            Query = resolver.Resolve<AppQuery>();
        }
    }
```
# Schema Types

## Scalars

GraphQL มีการระบุข้อมูลคล้ายๆกับการระบุข้อมูลของ C# โดยจะแบ่งตามตารางดังนี้

>scalars นี้จัดทำโดย [GraphQL Specification](https://graphql.github.io/graphql-spec/June2018/#sec-Scalars).

| GraphQL     | GraphQL .NET        | .NET                    |
|-------------|---------------------|-------------------------|
| `String`    | `StringGraphType`   | `string`                |
| `Int`       | `IntGraphType`      | `int`                   |
| `Float`     | `FloatGraphType`    | `double`                |
| `Boolean`   | `BooleanGraphType`  | `bool`                  |
| `ID`        | `IdGraphType`       | `int`, `long`, `string` |

> Note สามารถใช้ `Guid`ร่วมกับ `ID`ได้.  จะถูกแปลงให้เป้น `string` และจะส่งไปให้กับ GraphQL ในรูปแบบ `string`.


| GraphQL          | GraphQL .NET                    | .NET               |
|------------------|---------------------------------|--------------------|
| `Date`           | `DateGraphType`                 | `DateTime`         |
| `DateTime`       | `DateTimeGraphType`             | `DateTime`         |
| `DateTimeOffset` | `DateTimeOffsetGraphType`       | `DateTimeOffset`   |
| `Seconds`        | `TimeSpanSecondsGraphType`      | `TimeSpan`         |
| `Milliseconds`   | `TimeSpanMillisecondsGraphType` | `TimeSpan`         |
| `Decimal` | `DecimalGraphType` | `decimal` |
| `Uri` | `UriGraphType` | `Uri` |
| `Guid` | `GuidGraphType` | `Guid` |
| `Short` | `ShortGraphType` | `short` |
| `UShort` | `UShortGraphType` | `ushort` |
| `UInt` | `UIntGraphType` | `uint` |
| `Long` | `LongGraphType` | `long` |
| `ULong` | `ULongGraphType` | `ulong` |
| `Byte` | `ByteGraphType` | `byte` |
| `SByte` | `SByteGraphType` | `sbyte`
| `BigInt` | `BigIntGraphType` | `BigInteger`

Lists ของข้อมูลที่สนับสนุน Scalar หรือ Object .

| GraphQL    | GraphQL .NET                        | .NET           |
| -----------|-------------------------------------|----------------|
| `[String]` | `ListGraphType<StringGraphType>`    | `List<string>` |
| `[Boolean]` | `ListGraphType<BooleanGraphType>`    | `List<bool>` |

โดยการกำหนดใน code จะมีลักษณะดังนี้ (อ้างอิงจาก Object ที่มีใน Model)
```c#
public class UserType : ObjectGraphType<userModel>
{
    public UserType()
    {
        Field(x => x._id).Description("ID user");
        Field(x => x.index).Description("index of user");
        Field(x => x.guid).Description("GUID of user");
        Field(x => x.isActive).Description("User is Active ?");
        Field(x => x.balance).Description("balance in account");
        Field(x => x.picture).Description("picture of user");
        Field(x => x.age).Description("age");
        Field(x => x.eyeColor).Description("eyeColor");
        Field(x => x.name).Description("name");
        Field(x => x.subname).Description("sub name");
        Field(x => x.company).Description("company");
        Field(x => x.email).Description("email");
        Field(x => x.phone).Description("phone number");
        Field(x => x.address).Description("address");
        Field(x => x.registered).Description("registered");
        Field(x => x.latitude).Description("latitude of address");
        Field(x => x.longitude).Description("longitude of address");
        Field(x => x.greeting).Description("api is say ...");
        Field(x => x.favoriteFruit).Description("favorite Fruit");
    }
}
```