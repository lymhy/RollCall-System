在最新代码中，导入名单功能由 nameFileInput 的 change 事件处理，逻辑如下：
固定学号和序号模式（fixedIdRadio.checked）：
期望输入文件格式：学号,序号,名字（例如，“2021001,A01,张三”）。

支持部分空值：
学号可为空（例如，“,A01,张三”）。

序号可为空（例如，“2021001,,张三”）。

名字必填（至少包含名字）。

解析逻辑：
如果行包含 3 列及以上：studentId = parts[0], serialNo = parts[1], name = parts.slice(2).join(',')。

如果行包含 2 列：studentId = parts[0], name = parts[1], serialNo = ''。

如果行包含 1 列：name = parts[0], studentId = '', serialNo = ''。

学号唯一性检查：如果 studentId 非空，检查是否在 importedNames 中重复。

错误处理：跳过格式错误的行（无名字），提示用户（例如，“第X行格式错误”）。

自动递增序号模式（randomIdRadio.checked）：
期望输入文件格式：名字 或 学号,名字（例如，“张三” 或 “2021001,李四”）。

解析逻辑：
如果行包含 2 列及以上：studentId = parts[0], name = parts.slice(1).join(','), serialNo = ''。

如果行包含 1 列：name = parts[0], studentId = '', serialNo = ''。

学号唯一性检查：同固定模式。

错误处理：同固定模式。

输出：
解析后的记录存储到 importedNames 数组，格式为 { studentId, serialNo, name }。

调用 renderTable 更新表格，显示序号（serialNo 或 index + 1）、学号和姓名。