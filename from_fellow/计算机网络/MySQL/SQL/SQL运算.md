## SQL运算符

1. 算数运算符与其他语言无差别。
2. 比较运算符：
   - <>和!=的功能类似，但是不能比较值为NULL的数值
   - <=>安全的不等于比较，操作的值为NULL也能正确比较。
   - BETWEEN: a BETWEEN min AND max，当a大于等于min且小于等于max返回值为1，否则返回0。
   - IN: a IN (value1,value2...)当a的值在列表中时返回1。
   - IS NULL 与 IS NOT NULL，返回值是否是NULL。
   - LIKE： a LIKE %123% 当a中含有字符串123时，返回值1
   - REGEXP： str REGEXP str_pat 当str中含有与str_pat相匹配的字符串时返回1。

