### xlrd模块统计excel表格
打开excel表格
```
excel_obj = xlrd.open_workbook("oa_new.xlsx")
```
选择工作表
```
# 按序号选择
table = excel_obj.sheets()[0]
table = excel_obj.sheet_by_index(0)
```
完整的流程
```
excel_obj = xlrd.open_workbook("oa_new.xlsx")
table = excel_obj.sheet_by_index(0)
table.row_values(0)     #获取表的第一行的所有数据，返回列表
table.nrows   # 获取表的行数
table.ncols   # 获取表的列数
table.cell(0,0).value     # 获取单元格
```

### csv模块
```
import csv
# 读
reader = csv.reader(open("ssh_table.csv"), delimiter=',')   # reader就是所有行的迭代器，delimiter是行分割符，循环每一行得到每一行的所有数据，返回列表
for row in reader:
    print(row[0])

# 写
writer = csv.writer(open('bbb.csv', "w"), delimiter=",".encode("gb2312") )
writer.writerow(["name", "ddddd"])
```

### xlwt写入excel

```
wb = xlwt.Workbook()
ws = wb.add_sheet('A Test Sheet')  # 添加工作表

ws.write(0, 0, 1234.56)   # 在表格A1处写入1234.56
ws.write(1, 0, datetime.now())  # 在表格A2处写入当前日期的时间戳?
ws.write(2, 0, 1)
ws.write(2, 1, 1)
ws.write(2, 2, xlwt.Formula("A3*B3*5"))   # A3*B3*5的计算结果写入C3

wb.save('example.xls')
```

ws.write(行,列,内容,样式)

样式内容：
xlwt.easyxf(样式, 数据格式)
```
style1 = xlwt.easyxf(num_format_str='YYYY-mm-dd')
ws.write(1, 0, datetime.now(), style1)
```

单元格格式：
pattern 底纹
font 字体
border 边框
alignment 位置

```
pattern = xlwt.Pattern()
pattern.pattern = xlwt.Pattern.SOLID_PATTERN
pattern.pattern_fore_colour = 0x1f

style = xlwt.XFStyle()
style.pattern = pattern
style.font
style.border
style.alignment
style.num_format_str
```

设置行高列宽
```
wb = xlwt.Workbook()
ws = wb.add_sheet('A Test Sheet')

# 列宽
ws.col(0).width = 256 * 20

# 行高
tall_style = xlwt.easyxf('font:height 720;')
first_row = ws.row(0)
first_row.set_style(tall_style)
```

合并单元格
```
pattern = xlwt.Pattern()
pattern.pattern = xlwt.Pattern.SOLID_PATTERN
pattern.pattern_fore_colour = 0x1f
style = xlwt.XFStyle()
style.pattern = pattern

ws.write_merge(2, 4, 2, 4, "ffffffff", style)

wb.save('example.xls')
```
ws.write_merge(left_from, left_to, right_from, right_to, value, style)
















