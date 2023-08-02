# 接口数据参数校验

### 使用前景

在接口传输的到业务层时，往往需要对参数进行校验，然后做出判断。多个接口中可能存在重复的参数逻辑，为了解决这些冗余代码，可以通过引入`Dto`对象对参数做校验

以下主要结合class-validator、class-transformer、routing-controllers、reflect-metadata这些依赖性，通过装饰器去校验参数

### 依赖安装

``` shell
npm install routing-controllers
npm install reflect-metadata
npm install class-transformer class-validator
```

注：需要注意routing-controllers 与 class-transformer 和 class-validator 依赖的版本兼容


### 基础用法

创建Dto对象，并设定参数字段的校验规则
``` ts
// ./dto/user.dto.ts
import { IsString, MaxLength } from 'class-validator'
export class UserDto {
    @isString({message: "名字必须为字符串"})
    name: string;
}
```

业务层去校验数据
``` ts
// ./dto/user.controller.ts
import { validate } from 'class-validator'
import { plainToClass } from 'class-transformer';

export class UserController {

    // routing-controllers 对Body装饰器支持校验，可以直接通过注入的方式去自动校验
    @Post("/create")
    async create(@Body({validate: true}) user: UserDto ) {

    }

    // routing-controllers 对BodyParam，需要将装参数转为Dto实体，手动去校验
    @Post("/create2")
    async create2(@BodyParam() name: string ) {
       const userDto: UserDto = plainToClass(UserDto, { name })
        const errors = await validate(userDto);
        if (errors.length > 0) {
            // 参数校验失败，抛出错误
            throw new Error("Validation failed!");
        }
    }
}
```

### class-validator 校验方法

##### 常用的数据校验和业务数据装饰器

- @IsEmpty() 如果数据（=== '', === null, === undefined）则报错
- @IsIn(values: any[]) 如果values不包含数据则报错
- @IsIP(version?: "4"|"6") 校验数据是ipv4的地址还是ipv6的地址
...

更多请查看 [class-validator校验装饰器](https://github.com/typestack/class-validator#validation-decorators)

##### 自定义校验对象
创建校验对象
``` ts
import { ValidatorConstraint, ValidatorConstraintInterface, ValidationArguments } from 'class-validator';
@ValidatorConstraint({ name: 'customText', async: false })
export class CustomTextLength implements ValidatorConstraintInterface {
  validate(text: string, args: ValidationArguments) {
    return text.length > 1 && text.length < 10;
  }

  defaultMessage(args: ValidationArguments) {
    // here you can provide default error message if validation failed
    return 'Text ($value) is too short or too long!';
  }
}
```
校验对象应用，需要用过`Validate`装饰器应道Dto对象属性上 
``` ts
import { Validate } from 'class-validator';
import { CustomTextLength } from './CustomTextLength';

export class Post {
  @Validate(CustomTextLength, {
    message: 'Title is too short or long!',
  })
  title: string;
}
```

##### 自定义校验装饰器
注册校验装饰器
``` ts
import { registerDecorator, ValidationOptions, ValidationArguments } from 'class-validator';

export function IsLongerThan(property: string, validationOptions?: ValidationOptions) {
  return function (object: Object, propertyName: string) {
    registerDecorator({
      name: 'isLongerThan',
      target: object.constructor,
      propertyName: propertyName,
      constraints: [property],
      options: validationOptions,
      validator: {
        validate(value: any, args: ValidationArguments) {
          const [relatedPropertyName] = args.constraints;
          const relatedValue = (args.object as any)[relatedPropertyName];
          return typeof value === 'string' && typeof relatedValue === 'string' && value.length > relatedValue.length; 
        },
      },
    });
  };
}
```
使用装饰器
``` ts
import { IsLongerThan } from './IsLongerThan';

export class Post {
  title: string;

  @IsLongerThan('title', {
    message: 'Text must be longer than the title',
  })
  text: string;
}
```
