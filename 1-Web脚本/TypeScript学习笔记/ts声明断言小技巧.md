## ts声明断言小技巧



### 1、用来强制转换类型

# 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```ts
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为`as`语法：

```ts
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在TypeScript里使用JSX时，只有 `as`语法断言是被允许的。

> 上面是官网关于 类型断言 的说明

我在antd的upload组件里面发现了很好的一个实践。

```js
// upload.tsx
const targetItem = fileToObject(file);

// utils.tsx
export function fileToObject(file: RcFile): UploadFile {
  return {
    ...file,
    lastModified: file.lastModified,
    lastModifiedDate: file.lastModifiedDate,
    name: file.name,
    size: file.size,
    type: file.type,
    uid: file.uid,
    percent: 0,
    originFileObj: file,
  } as UploadFile;
}

// interface.tsx
export interface RcFile extends File {
  uid: string;
  readonly lastModifiedDate: Date;
  readonly webkitRelativePath: string;
}

export interface UploadFile<T = any> {

  uid: string;

  size: number;

  name: string;

  fileName?: string;

  lastModified?: number;

  lastModifiedDate?: Date;

  url?: string;

  status?: UploadFileStatus;

  percent?: number;

  thumbUrl?: string;

  originFileObj?: File | Blob;

  response?: T;

  error?: any;

  linkProps?: any;

  type: string;

  xhr?: T;

  preview?: string;

}
```

> 通过fileToObject方法，成功的将file（原本是object）转换成为了File类型
>
> 重点是 interface UploadFile，算了，我需要实践一下再说。