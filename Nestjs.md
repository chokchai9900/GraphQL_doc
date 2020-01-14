# 🐱 Nest JS & GraphQL 📚

### ติดตั้ง Nest JS

- ##### คำสั่งติดตั้ง Nest JS\_
        $npm i -g @nestjs/cli

### สร้าง project

- ##### คำสั่งสร้าง Project\_
        $nest new project-name

##### 📌เลือกการติดตั้งเป็นแบบ >npm

##### ✏️ ที่มา: https://docs.nestjs.com/

### ติดตั้ง และ เรียกใช้งาน GraphQL

#### การติดตั้ง Graph QL

- ##### คำสั่งติดตั้ง GraphQL

        $npm i --save @nestjs/graphql apollo-server-express graphql-tools graphql

* ##### import Graph QL ใน _app.module.ts_ ที่อยู่ใน Folder _src_

            import { Module } from '@nestjs/common';
            import { AppController } from './app.controller';
            import { AppService } from './app.service';
            import { GraphQLModule } from '@nestjs/graphql';
            import { CatsModule } from './cats/cats.module';

            @Module({
                imports: [
                    GraphQLModule.forRoot({}),
                ],
                controllers: [AppController],
                providers: [AppService],
            })
            export class AppModule {}

- ##### ใช้คำสั่ง

        $ npm i type-graphql

* ##### เพิ่ม _autoSchemaFile: 'schema.gql'_ ใน _GraphQLModule.forRoot_

        import { Module } from '@nestjs/common';
        import { AppController } from './app.controller';
        import { AppService } from './app.service';
        import { GraphQLModule } from '@nestjs/graphql';
        import { CatsModule } from './cats/cats.module';

        @Module({
            imports: [
                CatsModule,
                GraphQLModule.forRoot({
                autoSchemaFile: 'schema.gql',
                }),
            ],
            controllers: [AppController],
            providers: [AppService],
        })
        export class AppModule {}

### 💡 เรียกใช้งาน Graph QL และ ทดลองใช้งาน Graph QL 💡

- ##### สร้างโฟล์เดอร์ Cats ในโฟล์เดอร์ src
- ##### สร้างไฟล์ cats.resolver.ts ในโฟล์เดอร์ Cats
- ##### สร้างไฟล์ cats.module.ts ในโฟล์เดอร์ Cats

  ![Screen Shot 2020-01-13 at 10 44 27 PM](https://user-images.githubusercontent.com/46476206/72269938-f8f34000-3656-11ea-863b-0584d41705fc.png)

- ##### เพิ่ม code ในไฟล์ cat.resolver.ts
        import { Resolver, Query } from '@nestjs/graphql';
        @Resolver()
        export class CatsResolver {
            @Query(() => String)
            async hello() {
                return 'hello';
            }
        }
- ##### เพิ่ม code ในไฟล์ cats.module.ts

        import { Module } from '@nestjs/common';
        import { GraphQLModule } from '@nestjs/graphql';
        import { CatsResolver } from './cats.resolver';

        @Module({
            providers: [CatsResolver],
        })
        export class CatsModule {}

- ##### import CatsModule ในไฟล์ app.module.ts

        import { Module } from '@nestjs/common';
        import { AppController } from './app.controller';
        import { AppService } from './app.service';
        import { GraphQLModule } from '@nestjs/graphql';
        import { CatsModule } from './cats/cats.module';

        @Module({
            imports: [
                CatsModule,
                GraphQLModule.forRoot({
                autoSchemaFile: 'schema.gql',
                }),
            ],
            controllers: [AppController],
            providers: [AppService],
        })
        export class AppModule {}

* ##### เปิด Terminal เพื่อทดสอบใช้งาน Graph QL

        npm run start:dev

* ##### เปิด web browser แล้วใส่ URL:
        http://localhost:3000/graphql

- ##### ใส่ค่าในช่องด้านซ้าย จะได้ค่าตามภาพ

  ![Screen Shot 2020-01-13 at 11 10 06 PM](https://user-images.githubusercontent.com/46476206/72271679-e9292b00-3659-11ea-9efd-464ecbb9efe4.png)

##### ✏️ ที่มา:https://docs.nestjs.com/graphql/quick-start

## 🔑 ติดตั้ง Mongo DB

- ##### ใช้คำสั่งติดตั้ง Mongo DB
        $ npm install --save @nestjs/mongoose mongoose

* ##### import _MongooseModule_ ใน app.module.ts ที่อยู่ใน Folder src

          import { Module } from '@nestjs/common';
          import { AppController } from './app.controller';
          import { AppService } from './app.service';
          import { GraphQLModule } from '@nestjs/graphql';
          import { CatsModule } from './cats/cats.module';
          import { MongooseModule } from '@nestjs/mongoose';

          @Module({
              imports: [
                  CatsModule,
                  GraphQLModule.forRoot({
                  autoSchemaFile: 'schema.gql',
                  }),
                  MongooseModule.forRoot('mongodb://localhost/nest'),
              ],
              controllers: [AppController],
              providers: [AppService],
          })
          export class AppModule {}

  ###### ปล. _mongodb://localhost/nest_ คือ connection string ให้เปลี่ยน Connection string ที่เป็นของเรา

## 🔑 ติดต่อฐานข้อมูล Mongo DB

- ##### สร้างไฟล์ cats.schema.ts ในโฟลเดอร์ Cats และเพิ่มโค้ดในไฟล์ cats.schema.ts

        import * as mongoose from 'mongoose';

        export const CatSchema = new mongoose.Schema({
        name: String,
        age: Number,
        breed: String,
        });

- ##### import MongooseModule และ CatSchema ในไฟล์ cats.module.ts

        import { Module } from '@nestjs/common';
        import { GraphQLModule } from '@nestjs/graphql';
        import { CatsResolver } from './cats.resolver;
        import { MongooseModule } from '@nestjs/mongoose';
        import { CatSchema } from './cats.schema';

        @Module({
            imports: [MongooseModule.forFeature([{ name: 'Cat', schema: CatSchema }])],
            providers: [CatsResolver],
        })
        export class CatsModule {}

- ##### สร้างไฟล์ cats.service.ts ในโฟลเดอร์ Cats และเพิ่มโค้ดในไฟล์ cats.service.ts

        import { Model } from 'mongoose';
        import { Injectable } from '@nestjs/common';
        import { InjectModel } from '@nestjs/mongoose';
        import { Cat } from './interfaces/cat.interface';
        import { CatInput } from './inputs/cat.input';

        @Injectable()
        export class CatsService {
            constructor(@InjectModel('Cat') private readonly catModel: Model<Cat>) {}

            async create(createCatDto: CatInput): Promise<Cat> {
                const createdCat = new this.catModel(createCatDto);
                return await createdCat.save();
            }

            async findAll(): Promise<Cat[]> {
                return await this.catModel.find().exec();
            }
        }

* ##### สร้างโฟลเดอร์ interfaces ในโฟลเดอร์ Cats และสร้างไฟล์ cats.interface.ts

  ![Screen Shot 2020-01-13 at 11 52 52 PM](https://user-images.githubusercontent.com/46476206/72275337-49bb6680-3660-11ea-976f-1e958311f3bb.png)

* ##### เพิ่มโค้ดในไฟล์ cats.interface.ts

        import { Document } from 'mongoose';

        export interface Cat extends Document {
            readonly id: string;
            readonly name: string;
            readonly age: number;
            readonly breed: string;
        }

- ##### สร้างโฟลเดอร์ dto ในโฟลเดอร์ Cats และสร้างไฟล์ create-cat.dto.ts

  ![Screen Shot 2020-01-14 at 12 05 40 AM](https://user-images.githubusercontent.com/46476206/72276034-abc89b80-3661-11ea-8bb2-6a3f7bb58549.png)

- ##### เพิ่มโค้ดในไฟล์ create-cat.dto.ts

        import { ObjectType, Field, Int } from 'type-graphql';

        @ObjectType()
        export class CreateCatDto {
            @Field(()=>ID)
            id:string;
            @Field()
            readonly name: string;
            @Field(() => Int)
            readonly age: number;
            @Field()
            readonly breed: string;
        }

* ##### import CatsService ในส่วนของ Providers ใน cats.module.ts

        import { Module } from '@nestjs/common';
        import { GraphQLModule } from '@nestjs/graphql';
        import { CatsResolver } from './cats.resolver';
        import { MongooseModule } from '@nestjs/mongoose';
        import { CatSchema } from './cats.schema';
        import { CatsService } from './cats.service';

        @Module({
            imports: [MongooseModule.forFeature([{ name: 'Cat', schema: CatSchema }])],
            providers: [CatsResolver,CatsService],
        })
        export class CatsModule {}

* ##### เพิ่ม CatsService ในส่วนของ constructor ใน หน้า cats.resolver

        import { Resolver, Query } from '@nestjs/graphql';
        import { CatsService } from './cats.service';
        @Resolver()
        export class CatsResolver {
            constructor(
                private readonly catsService:CatsService,
            ){}
            @Query(() => String)
            async hello() {
                return 'hello';
            }
        }

* ##### เพิ่ม Function @Query เพื่อดึงข้อมูล และ Function @Matation สร้างข้อมูลใหม่ ในหน้า CatsService


        import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
        import { CatsService } from './cats.service';
        import { CreateCatDto } from './dto/create-cat.dto';

        @Resolver()
        export class CatsResolver {
        constructor(private readonly catsService: CatsService) {}
        @Query(() => String)
        async hello() {
            return 'hello';
        }

        @Query(() => [CreateCatDto])
        async cats() {
            return this.catsService.findAll();
        }

        @Mutation(() => CreateCatDto)
        async createCat(@Args('input') input: CatInput) {
            return this.catsService.create(input);
        }
        }

- ##### สร้างโฟลเดอร์ inputs ในโฟลเดอร์ cats จากนั้นสร้างไฟล์ inputs

  ![Screen Shot 2020-01-14 at 12 47 14 AM](https://user-images.githubusercontent.com/46476206/72278864-7aeb6500-3667-11ea-9cc6-cf4c835d0d0c.png)

- ##### เพิ่มโค้ดในไฟล์ cat.input.ts

        import { InputType, Field, Int } from 'type-graphql';

        @InputType()
        export class CatInput {
            @Field()
            readonly name: string;
            @Field(() => Int)
            readonly age: number;
            @Field()
            readonly breed: string;
        }

* ##### เปิด Terminal รันอีกครั้ง

        npm run start:dev

- ##### เปิด web browser แล้วใส่ URL:
        http://localhost:3000/graphql

* ##### ทดสอบเพิ่มข้อมูลลง database โดยใช้ Graph QL

        mutation{
            createCat(input:{name:"bob2",age:2,breed:"g"}){
                id
                name
                age
                breed
            }
        }

  ###### ตามรูปภาพ ข้อมูลก็จะถูกเพิ่มลง database

  ![Screen Shot 2020-01-14 at 10 15 20 AM](https://user-images.githubusercontent.com/46476206/72311331-d6910f00-36b6-11ea-80d4-2a929ae09626.png)

- ##### ทดสอบดึงข้อมูลจาก database โดยใช้ Graph QL

        {
            cats{
                id
                name
                age
                breed
            }
        }

  ###### ตามรูปภาพ เป็นการดึงข้อมูลจาก database

  ![Screen Shot 2020-01-14 at 10 17 11 AM](https://user-images.githubusercontent.com/46476206/72311440-41dae100-36b7-11ea-99d8-01ff1f8ac8ad.png)

  ##### ✏️ ที่มา:https://docs.nestjs.com/techniques/mongodb

## 🙄🙄 ตัวอย่างโค้ดเพิ่มเติม 😃😃

####📌 หากต้องการเพ่ิม Function อื่น ให้เพิ่มที่หน้า Cats.Servic.ts

##### 🗝 Function Query ข้อมูล ด้วย ID

        async findSingleCat(id: string): Promise<Cat> {
            return await this.catModel.findOne({ _id: id });
        }

##### 🗝 Function update ข้อมูล

        async updateCat(id: string, cat: CatInput): Promise<Cat> {
            return await this.catModel.findByIdAndUpdate(id, cat, { new: true });
        }

##### 🗝 Function Delete ข้อมูล

        async deleteSingleCat(id: string): Promise<Cat> {
            return await this.catModel.findByIdAndRemove(id);
        }

### 💢 ตัวอย่าง 💢

![Screen Shot 2020-01-14 at 10 39 21 AM](https://user-images.githubusercontent.com/46476206/72312269-30470880-36ba-11ea-9938-d1a896e3a3e1.png)

##### จากนั้น หากต้องการเรียกใช้ function ที่เราได้สร้างขึ้นเพิ่มเติม ให้เราเรียกใช้ function ดังกล่าวที่หน้า cats.resolver.ts ดังรูปภาพดังกล่าว

### 💢 ตัวอย่าง 💢

![Screen Shot 2020-01-14 at 10 36 41 AM](https://user-images.githubusercontent.com/46476206/72312196-e9f1a980-36b9-11ea-824a-f2926f77b01a.png)

##### ✏️ ที่มา: https://gabrieltanner.org/blog/nestjs-graphqlserver
