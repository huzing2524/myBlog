### Django raw原生sql语句查询，%转义报错的问题
- 报错：TypeError: not enough arguments for format string, 这个“%”造成python认为此字符串有format倾向，出现错误。
- 解决办法：使用 %% 来转义 %
- 模糊查询：`Model.Objects.raw("select * from model where field like %{}%".format(condition))` 会报错，
应使用 `Model.Objects.raw("select * from model where field like %%{}%%".format(condition))`
- 日期格式化字符串: `%Y-%m-%d`