# python

> 类似地，可能不会系统学习一些语言，在共同性之下记录一些差异性

1. python中的三目运算符:
    - `expr1 if condition else expr2`,如`return a + b if b >= 0 else a - b`
    - c/java中则是`condition ? expr1 : expr2`

2. `match-case`(python的`switch-case`):
```
def http_status(code):
    match code:
        case 200:
            return "OK"
        case 404:
            return "Not Found"
        case 500:
            return "Server Error"
        case _:           # 相当于 C 里的 default
            return "Unknown"

print(http_status(200))  # OK
print(http_status(999))  # Unknown
```

- 不需要写 break，匹配成功后会直接退出，不会继续执行下一个 case。
- 可以使用 `|` 来匹配多个值：case 401 | 403 | 405:
- `case _` 是通配符，相当于 default。


