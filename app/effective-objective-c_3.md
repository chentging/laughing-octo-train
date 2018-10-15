Effective Objective-C: 3-多用字面量语法，少用与之等价的方法
===

## 字面数值 NSNumber

少用：

```oc
NSNumber *someNumber = [NSNumber numberWithInt:1];
```

建议：

```oc
NSNumber *someNumber = @1;
```

优点： 简洁

## 字面量数组 NSArray

少用：

```oc
NSArray *animals = [NSArray arrayWithObjects:@"cat", @"dog", @"mouse", @"badger", nil];
```

建议：

```oc
NSArray *animals = @[@"cat", @"dog", @"mouse", @"badger"];
```

**取下标**

```oc
NSString *dog = animals[1];
```

优点： 简洁，更易理解，更安全

`语法糖`，其效果等于是先创建一个数组，然后把反括号內的所有对象都加到这个数组中

## 字面量字典 NSDictionary

少用：

```oc
NSDictionary *personData =
    [NSDictionary dictionaryWithObjectsAndKey:
        @"Matt", @"firstName",
        @"Galloway", @"lastName",
        [NSNumber numberWithInt:28], @"age",
        nil];
```

建议：

```oc
NSDictionary *personData =
    @{@"firstName" : @"Matt",
      @"lastName" : @"Galloway",
      @"age" : @28};
```

**取下标**

```oc
NSString *lastName = personData[@"lastName"]
```

优点： 简洁，更易理解，更安全

## 可变数组与字典

少用：

```oc
[mutableArray replaceObjectAtIndex:1 withObject:@"dog"];
[mutableDictionary setObject:@"Galloway" forKey:@"lastName"];
```

建议：

```oc
mutableArray[1] = @"dog";
mutableDictionary[@"lastName"] = @"Galloway";
```

## 要点

- 应该使用字面量语法来创建字符串/数值/数组/字典。与创建此类对象的常规方法相比，这么做更加简明扼要。
- 应该通过取下标操作来访问数组下标或字典中的键所对应的元素。
- 用字面量语法创建数组或字典时，若值中有nil，则会抛出异常。因此，务必确保值里不含nil。
