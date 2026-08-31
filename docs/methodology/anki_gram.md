# anki中的语法笔记

> 由于本质是html编辑，所以html里的语法都还成立

## 调用filed

- 基础调用某个filed：`{{filed名称}}`
- 只有“name”这个 Field 有内容时，才显示中间这些东西:
    ```
    {{#name}}
    ...
    {{/name}}
    ```
    - 如果我们直接在frontside从头到尾先套一个如此模板，那么card就只会在有这个filed内容时生成，否则跳过
