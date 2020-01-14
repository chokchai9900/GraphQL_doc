# Advanced GraphQL in ASP.NET Core (Mutations , resolve)
Mutations คือการกระทำใดๆ ที่ทำให้ข้อมุลใน database มีการเปลี่ยนแปลง (POST, PUT, DELETE Actions)

การทำ Mutations มีสองส่วนหลัก คือ
- request
- query variables

## กำหนด input สำหรับการ Mutations

```c#
public class UserInputType : InputObjectGraphType
{
    public UserInputType()
    {
        Name = "userInput";
        Field<NonNullGraphType<IntGraphType>>("index");
        Field<NonNullGraphType<StringGraphType>>("guid");
        Field<NonNullGraphType<BooleanGraphType>>("isActive");
        Field<NonNullGraphType<StringGraphType>>("balance");
        Field<NonNullGraphType<StringGraphType>>("picture");
        Field<NonNullGraphType<IntGraphType>>("age");
        Field<NonNullGraphType<StringGraphType>>("eyeColor");
        Field<NonNullGraphType<StringGraphType>>("name");
        Field<NonNullGraphType<StringGraphType>>("subname");
        Field<NonNullGraphType<StringGraphType>>("company");
        Field<NonNullGraphType<StringGraphType>>("email");
        Field<NonNullGraphType<StringGraphType>>("phone");
        Field<NonNullGraphType<StringGraphType>>("address");
        Field<NonNullGraphType<StringGraphType>>("registered");
        Field<NonNullGraphType<StringGraphType>>("latitude");
        Field<NonNullGraphType<StringGraphType>>("longitude");
        Field<NonNullGraphType<StringGraphType>>("greeting");
        Field<NonNullGraphType<StringGraphType>>("favoriteFruit");
    }
}
```

## กำหนด Schema 
```c#
public class AppSchema : Schema
    {
        public AppSchema(IDependencyResolver resolver)
            :base(resolver)
        {
            Query = resolver.Resolve<AppQuery>();
            Mutation = resolver.Resolve<AppMutation>(); // ส่วนที่เพิ่มเข้าไป
        }
    }
```
## เพิ่ม method สำหรับการ Mutations
ทำการเพิ่ม method สำหรับ Update ข้อมูล ใน service ของเรา
```c#
public void UpdateUserByID(userModel baseUser, userModel UserInput)
{
    UserInput._id = baseUser._id;
    _user.ReplaceOne(it => it._id == baseUser._id, UserInput);
}
```
เมื่อทำการเพิ่มเสร็จแล้ว ให้ทำการสร้าง class สำหรับการ Mutations ขึ้นมา ตามตัวอย่าง

```c#
public class AppMutation : ObjectGraphType
{
    Field<UserType>
    (
        name : "updateUser",
        arguments: new QueryArguments(
            new QueryArgument<NonNullGraphType<UserInputType>> { Name = "user" },
            new QueryArgument<NonNullGraphType<StringGraphType>> { Name = "userId" }
            ),
        resolve: context =>
        {
        var user = context.GetArgument<userModel>("user");
        var userId = context.GetArgument<string>("userId");

        var baseUser = dBService.GetUserByID(userId);
        if (baseUser == null)
        {
            context.Errors.Add(new ExecutionError("Couldn't find user in db."));
                return null;
        }
        dBService.UpdateUserByID(baseUser, user);
        return user;
        }
    );
}
```

ผลลัพท์ที่ได้จะมีดังนี้

ก่อน Update

![GitHub Logo](ImageLink)


หลัง Update

![GitHub Logo](ImageLink)