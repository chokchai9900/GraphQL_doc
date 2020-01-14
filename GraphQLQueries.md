# GraphQL Queries in ASP.NET Core
Queries คือการ นำข้อมุล โดยมี requirement เป็นตัวระบุข้อมุลว่า ต้องการข้อมูล Field ใดบ้าง

ตัวอย่างเช่น :
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
## สิ่งที่ต้องมีก่อนเริ่มทำ
- Models ของข้อมูลที่จะทำงานด้วย
- Service ที่เชื่อมต่อกับ Database แล้ว
- Packet
    - GraphQL
    ```
    PM> Install-Package GraphQL -Version 2.4.0
    ```
    - GraphQL.Server.Transports.AspNetCore
    ```
    PM> Install-Package GraphQL.Server.Transports.AspNetCore -Version 3.4.0
    ```
    - GraphQL.Server.Ui.Playground
    ```
    PM> Install-Package GraphQL.Server.Ui.Playground -Version 3.4.0
    ``` 

## Let's Start !!

- สร้าง Schema
```c#
public class AppSchema : Schema
{
    public AppSchema(IDependencyResolver resolver)
        :base(resolver)
    {
 
    }
}
``` 

- สร้าง Type เพื่อรองรับข้อมูล
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

- Libraries and Schema Registration

เพิ่มโค๊ดส่วนนี้เ เพื่อทำการ Registration กับตัวโปรแกรม
```c#
public void ConfigureServices(IServiceCollection services){
    services.AddScoped<IDependencyResolver>(s => new FuncDependencyResolver(s.GetRequiredService));
    services.AddScoped<AppSchema>();
    services.AddScoped<DBService>();

    services.AddGraphQL(o => { o.ExposeExceptions = false; })
            .AddGraphTypes(ServiceLifetime.Scoped);
}
```

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env){
    app.UseGraphQL<AppSchema>();
    app.UseGraphQLPlayground(options: new GraphQLPlaygroundOptions());
}
```

เมื่อทำเสร็จ ให้ลองรับโปรแกรม แล้วเข้าลิ้งค์ https://localhost:5001/ui/playground เพื่อเข้าสู่ UI ของ GraphQL

![BeginGraphQL](Imahttps://github.com/chokchai9900/GraphQL_doc/blob/master/picture/start.PNGge)

แล้วลอง Queries ข้อมูล โดยใช้ Querie string นี้
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

ผลลัพท์ที่ได้จะได้แบบนี้ 

![Get](https://github.com/chokchai9900/GraphQL_doc/blob/master/picture/get.PNG)

