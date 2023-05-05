# Linux 命令——find

> find 命令是 Linux 系统中用于查找文件和目录的强大工具。该命令可以按文件名、文件类型、文件大小、修改时间、所有者、权限等多种条件进行搜索，非常灵活。下面列出了 find 命令的常用选项和用法。

### 基本语法

```
find 路径 [选项] [操作]
```

`路径`指定要搜索的起始目录，可以是绝对路径或相对路径，也可以是多个路径。`选项`用于指定搜索条件，`操作`则用于对搜索结果进行操作，如打印、删除、复制等。

### 常用选项

#### -name：按文件名进行搜索

可使用通配符去匹配文件目录

- \*：匹配任意字符，包括空字符
- ?：匹配任意单个字符，不包括空字符
- []：匹配方括号中列举的任意一个字符，例如[abc]将会匹配 a、b 或者 c 这三个字符
- [!]：匹配方括号中未列举的任意一个字符，例如[!abc]将会匹配除了 a、b、c 之外的任意一个字符
- \：转义字符，用于匹配特殊字符本身，例如\*、?等

```shell
# 搜索/etc目录中所有以.conf结尾的文件
find /etc -name "*.conf"
# 搜索所有以.txt结尾的文件
find /path/to/search -type f -name "*.txt"
# 搜索所有*.txt的文件
find /path/to/search -type f -name "\*.txt"
# 搜索所有以.TXT结尾的文件，不区分大小写
find /path/to/search -type f -iname "*.TXT"
# 搜索所有文件名为file1.txt、file2.txt、file3.txt的文件
find /path/to/search -type f -name "file[123].txt"
# 搜索除了文件名为file1.txt、file2.txt、file3.txt以外所有file[*].txt文件
find /path/to/search -type f -name "file[!123].txt"
```

注：如果想匹配时不区分大小写可以用`-iname` 代替`-name`

#### -type：按文件类型进行搜索

常用类型包括 f（普通文件）、d（目录）、l（符号链接）、b（块设备）、c（字符设备）等

```shell
# 搜索当前目录及其子目录中的所有普通文件
find . -type f
```

#### -size：按文件大小进行搜索

可以使用+或-来指定文件大小的范围，还可以使用 c、k、M、G 等后缀来表示不同的单位（字节、千字节、兆字节和千兆字节）

```shell
# 搜索/home目录中大小超过1MB的所有文件
find /home -size +1M

# 搜索/home目录中大小不超过1kb的所有文件
find /home -size -1k

# 搜索/home目录中大小在100KB和1MB之间的文件, 通过-and符号组合，-and 符号可省略
find /home -size +100k -size -1M
find /home -size +100k -and -size -1M

# 搜索/home目录中小于100KB或大于1MB的文件, 通过-or符号组合，可简写-o。
# 需要将组合命令用括号起来
find /home \( -size +100k -o -size +1M \)
```

#### -mtime：按文件修改时间进行搜索

其语法为-mtime [+-]n，其中 n 为天数，+表示大于等于该天数，-表示小于等于该天数。还有-atime 和-ctime 选项可以用于按照文件访问时间和改变时间进行搜索，其语法和-mtime 选项类似

```shell
# 查找在过去7天之前修改过的文件
find /path/to/search -type f -mtime +7
# 查找在过去7天内访问过的文件
find /path/to/search -type f -atime -7
```

#### -user：按文件所有者进行搜索

其语法为`-user username`，其中 `username` 为指定的用户名

如果要查找所有不属于标记用户的文件，可以在 `username` 前加上`!`

```shell
# 查找所有属于john用户的文件
find /path/to/search -type f -user john
# 查找所有不属于john用户的文件
find /path/to/search -type f ! -user john
# 查找所有属于admins组的文件
find /path/to/search -type f -group admins
```

注：如需按用户组查找用`-group`替换`-user`，语法一致

#### -perm：按文件权限进行搜索。可以使用/指定权限位

其语法为-perm mode，其中 mode 是一个三位数的权限模式，可以使用八进制或符号表示法指定

如果要查找所有具有至少 `mode` 权限的文件，可以在 `mode` 前加上`/`

```shell
# 查找所有具有644权限的文件
find /path/to/search -type f -perm 644
# 查找所有具有至少644权限的文件，可以在mode前加上/
find /path/to/search -type f -perm /644
```

### 高级选项

> 注：高级选项命令在于常规选项并用的时候建议将选项放在选项位置的最前端，虽然不影响结果
> ，但是系统会警告异常

#### -depth：按深度优先搜索

用于按照深度优先的顺序搜索文件，即先搜索子目录中的文件，再搜索当前目录中的文件

```shell
find /path/to/search -depth -type f -name "*.txt"
```

该命令将会搜索/path/to/search 目录下的所有文件，并返回所有扩展名为.txt 的文件，而且会先搜索子目录中的文件，再搜索当前目录中的文件。如果不加-depth 选项，则会按照默认的广度优先顺序搜索文件

#### -maxdepth：指定最大搜索深度

用于限制搜索目录结构的深度，即指定最大搜索深度。其语法为-maxdepth level，其中 level 是一个数字，表示最大搜索深度

```shell
find . -maxdepth 2 -type f -name "*.txt"
```

该命令会从当前目录开始搜索，最多搜索到两级目录（即当前目录及其一级子目录），返回所有扩展名为.txt 的文件

#### -mindepth：指定最小搜索深度

用于限制搜索目录结构的最小深度，即指定最小搜索深度。其语法为-mindepth level，其中 level 是一个数字，表示最小搜索深度

```shell
find . -mindepth 1 -type f -name "*.txt"
```

该命令会从当前目录开始搜索，但最小搜索深度为 1，即不搜索当前目录，只搜索子目录。返回所有扩展名为.txt 的文件

#### -newer：按修改时间进行搜索，但是指定的文件必须比搜索结果新

用于查找修改时间比指定文件更新的文件。其语法为-newer file，其中 file 是一个文件名，表示与其比较修改时间的文件

```shell
find . -type f -newer test.txt
```

该命令会从当前目录开始搜索，返回所有修改时间比 test.txt 更新的文件，包括子目录中的文件。如果 test.txt 不存在，则该命令不会匹配任何文件

#### -regex：使用正则表达式进行搜索

用于基于正则表达式匹配文件名进行查找。其语法为-regex pattern，其中 pattern 是一个正则表达式，表示要匹配的文件名模式

```shell
find . -type f -regex '.*\.txt$'
```

该命令会从当前目录开始搜索，返回所有以.txt 结尾的文件，包括子目录中的文件。其中，.\*表示任意字符任意次，\.表示匹配.字符，$表示匹配字符串结尾

#### -prune：剪枝操作

用于于剪枝操作，即跳过指定目录或文件不进行搜索

通常情况下，它和`-o`选项一起使用，用于排除某些目录或文件，以便在剩下的目录或文件中进行搜索

```shell
# 查询当前目录下除了node_module目录以外所有的文件
find ./ type d -name 'node_module' -prune -o -type -f
```

### 常规操作

> 操作命令需要在整个命令行的最后面，默认值为-print

#### -print：打印搜索结果

匹配到的文件名打印到标准输出（通常是终端屏幕）。如果没有指定操作命令，find 命令会自动使用 -print 命令

```shell
# 将结果显示在终端上，该操作命令为默认缺省值
find . -name "*.txt" -print
find . -name "*.txt"
```

#### -delete：删除搜索结果

用于删除匹配到的文件或目录

```shell
# 删除当前目录下所有扩展名为 .log 的文件
find . -name "*.log" -delete
```

注：<font color=red>**该删除操作不会询问是否确认删除操作**</font>

#### -exec：对搜索结果执行命令

用于对搜索到的文件或目录执行特定的命令。-exec 可以将搜索到的文件或目录作为参数传递给指定的命令，并对其进行操作

**需要在命令末尾加上`\`**

```shell
find /path/to/search -name "pattern" -exec command {} \;
```

`command` 可以为任何有效的 Linux 命令，`{}` 表示 find 命令匹配到的文件目录占位参数，`\` 表示结束符号

```shell
# 查找并删除所有的 .bak 文件
find . -name "*.bak" -exec rm -f {} \
# 查找并复制所有的 .txt 文件到 /tmp 目录
find . -name "*.txt" -exec cp {} /tmp \
# 查找并修改所有的 .sh 文件的权限
find . -name "*.sh" -exec chmod +x {} \
```

注：<font color=red>**操作不询问不可逆，谨慎使用**</font>

#### -ok

与-exec 类似，但在执行命令前需要确认，语法于-exec 也一致

```shell
find . -name "*.log" -ok rm {} \;
```

如上面的命令，在将查找到文件进行删除时，会进行逐个进行询问用户是否确认删除，用户输入 y 或 Y 会进行删除
