经常会有修改表中某字段的时间值，修改后的值为相对原时间的值，如+1 days, - 1 months这类的。下面代码用于直接生成对应的sql。

```python
    def get_update_date_sql_for_oracle(table_name,col_name_list,interval,unit,sign,condition):
        """根据所给的条件生成更改对应数据库对于字段的时间的sql，用于更改生效日sql
            参数:
                - table_name: 表名
                - col_name_list: 列名的list
                - interval: 时间值
                - unit: 时间单位:months,days
                - sign: +，-符号
                - condition: where后的条件语句

            例:
            | get update date sql for oracle | ${table_name} | ${col_name_list} | ${interval} | ${unit} | ${sign} | ${condition} |
            """
        return self.get_update_date_sql(table_name, col_name_list, interval, unit, sign, condition, "oracle")


    def get_update_date_sql_for_postgre(table_name,col_name_list,interval,unit,sign,condition):
        """根据所给的条件生成更改对应数据库对于字段的时间的sql，用于更改生效日sql
        参数:
            - table_name: 表名
            - col_name_list: 列名的list
            - interval: 时间值
            - unit: 时间单位:months,days
            - sign: +，-符号
            - condition: where后的条件语句

        例:
         | get update date sql for postgre | ${table_name} | ${col_name_list} | ${interval} | ${unit} | ${sign} | ${condition} |
        """
        return self.get_update_date_sql(table_name, col_name_list, interval, unit, sign, condition, "postgre")


    def get_update_date_sql( table_name, col_name_list, interval, unit, sign, condition, dbtype):
        """根据所给的条件生成更改对应数据库对于字段的时间的sql，用于更改生效日sql
               参数:
                   - table_name: 表名
                   - col_name_list: 列名的list
                   - interval: 时间值
                   - unit: 时间单位:months,days
                   - sign: +，-符号
                   - condition: where后的条件语句
                   - dbtype: 数据库类型，当前仅实现了oracle和postgre
         """
        set_sql = ""
        if not isinstance(col_name_list, list):
            raise Exception("col_name_list的类型必须是list")
        if len(col_name_list) == 0:
            raise Exception("col_name_list不可以为空list")
        if unit not in ['months', 'days']:
            raise Exception("unit的值只能是months或者days，当前是%s" % unit)
        i = 0

        for col_name in col_name_list:
            if dbtype=="postgre":
                set_sql += "%s=(%s %s interval '%d %s')" % (col_name, col_name, sign, int(interval), unit)
            elif dbtype=="oracle" and unit == "months":
                set_sql += "%s=add_months(%s,%s%d)" % (col_name, col_name, sign, int(interval))
            elif dbtype=="oracle" and unit == "days":
                 set_sql += "%s=%s%s%d" % (col_name, col_name, sign, int(interval))
            else:
                raise Exception("不支持的dbtype")

            i = i + 1
            if len(col_name_list) != i:
                set_sql = set_sql + ","

        result_sql = "update %s set %s where %s;" % (table_name, set_sql, condition)
        return result_sql

```