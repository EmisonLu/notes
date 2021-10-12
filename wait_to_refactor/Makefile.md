## Makefile

### 规则

* Makefile描述的是文件编译的相关规则，它的规则主要由两个部分组成，分别是依赖的关系和执行的命令，其结构如下所示：

  ```makefile
  targets : prerequisites
      command
  ```

  或者是

  ```makefile
  targets : prerequisites; command
      command
  ```

  * targets：规则的目标，可以是Object File（一般称它为中间文件），也可以是可执行文件，还可以是一个标签。

  * prerequisites：依赖文件，生成targets需要的文件或者目标。可以多个，也可以没有。

  * command：make需要执行的命令（任意的 shell 命令）。可以有多条命令，每一条命令占一行。

  * 例子：

    ```makefile
    test:test.c
        gcc -o test test.c
    ```

* 使用 Makefile 的方式：首先需要编写好 Makefile 文件，然后在 shell 中执行 make 命令，程序就会自动执行，得到最终的目标文件。

* Makefile的5个部分

  1. 显式规则：说明了如何生成一个或多个目标文件。由 Makefile 的书写者明显指出要生成的文件、依赖文件、生成的命令。
  2. 隐含规则：由于 make 命令有自动推导的功能，所以隐晦的规则允许比较简略地书写 Makefile，这是由 make 命令所支持的。
  3. 变量定义：在 Makefile 中要定义一系列的变量，变量一般都是字符串，类似于C语言中的宏，当 Makefile 被执行时，其中的变量都会被扩展到相应的引用位置上。
  4. 文件指示：3个部分
     1. 在一个 Makefile 中引用另一个 Makefile，类似于C语言中的 include。
     2. 根据某些情况指定 Makefile 中的有效部分，类似于C语言中的预编译 #if。
     3. 定义一个多行的命令。
  5. 注释：只有行注释，用“#”字符，类似于C中的“//”。如果要在 Makefile 中使用“#”字符，可以用反斜框进行转义，如：“\\#”。

* 在一个 Makefile 中使用指示符 "include" 包含另一个 Makefile 文件。

### 工作流程

* 执行 make 命令的时候，make 就会去 Makefile 文件下找要执行的编译规则，也可以使用的文件名称为 "GNUmakefile" 、"makefile" 。

* 例子：

  ```makefile
  main:main.o test1.o test2.o
  		gcc main.o test1.o test2.o -o main
  main.o:main.c test.h
  		gcc -c main.c -o main.o
  test1.o:test1.c test.h
  		gcc -c test1.c -o test1.o
  test2.o:test2.c test.h
  		gcc -c test2.c -o test2.o
  ```

  * 编译项目文件时，默认情况下，make 执行的是 Makefile 中的第一规则，此规则的第一目标称之为“最终目标”或者是“终极目标”。
  * 执行 make 命令，可以得到可执行文件 main 和中间文件 main.o、test1.o 和 test2.o，main 就是要生成的最终文件。通过 Makefile 可以发现，目标 "main"在 Makefile 中是第一个目标，因此它就是 make 的终极目标，当修改任何 C 文件后，执行 make 将会重建终极目标 main。
  * 对 ".o" 文件为目标的规则处理有三种情况：
    1. 目标 ".o" 文件不存在，使用其描述规则创建它。
    2. 目标 ".o" 文件存在，目标 ".o" 文件所依赖的 ".c" 源文件 ".h" 文件中的任何一个比目标 ".o" 文件更新（在上一次 make 之后被修改），则根据规则重新编译生成它。
    3. 目标 ".o" 文件存在，目标 ".o" 文件比它的任何一个依赖文件（".c" 源文件、".h" 文件）“更新”（它的依赖文件在上一次 make 之后没有被修改），则什么也不做。

* 中间文件（编译时生成的 ".o" 文件）的作用：检查某个源文件是不是进行过修改，最终目标文件是不是需要重建。执行 make 命令时，没有改变的文件不用重新编译，从而节省时间，提高编程效率。

* 在使用时产生中间文件会让整个文件看起来很乱，所以在编写 Makefile 文件的时候会在末尾加上这样的规则语句：

  ```makefile
  .PHONY:clean
  clean:
      rm -rf *.o test
  ```

  *  "*.o" 是执行过程中产生的中间文件，"test" 是最终生成的执行文件。clean 是独立的，它只是一个伪目标，不是具体的文件，不会与第一个目标文件相关联，所以在执行 make 的时候不会执行下面的命令。

### 通配符

* Makefile 可以使用 shell 命令， shell 支持的通配符在 Makefile 中同样适用。 shell 中使用的通配符有："*"，"?"，"[...]"。

  | 通配符 | 使用说明                       |
  | ------ | ------------------------------ |
  | *      | 匹配0个或者是任意个字符        |
  | ？     | 匹配任意一个字符               |
  | []     | 可以指定匹配的字符放在 "[]" 中 |

* 通配符可以出现在模式的规则中，也可以出现在命令中。

* 例子：

  ```makefile
  .PHONY:clean
  clean:
      rm -rf *.o test
  ```

  * 通配符可以使用在规则的命令当中，表示的是以 .o 结尾的任意文件。

* 例子：

  ```makefile
  test:*.c
      gcc -o $@ $^
  ```

  * 通配符可以使用在规则中，用来表示以 .c 结尾的所有文件。

* 以下使用有误：

  ```makefile
  OBJ=*.c
  test:$(OBJ)
      gcc -o $@ $^
  ```

  * 不能通过引用变量的方式来使用通配符，以上使用的时候并没有展开，而是直接识别成了一个名为 "*.c"的文件。

  * 使用函数 "wildcard"，可以解决以上问题：

    ```makefile
    OBJ=$(wildcard *.c)
    test:$(OBJ)
        gcc -o $@ $^
    ```

* 字符"%"：和通配符 "*" 类似，用于匹配任意个字符。

  ```makefile
  test:test.o test1.o
      gcc -o $@ $^
  %.o:%.c
      gcc -o $@ $^
  ```

  *  "%.o" 把所有 ".o" 文件组合成一个列表，从列表中逐个取出文件，"%" 表示取出来文件的文件名（不包含后缀），然后找到文件中和 "%" 名称相同的 ".c" 文件，然后执行下面的命令，直到列表中的文件全部被取出来为止。
  * 以上属于 Makefile 的静态模式规则：规则存在多个目标，不同的目标可以根据目标文件的名字来自动构造出依赖文件。

### 变量的定义和使用

* Makefile 文件中定义变量的基本语法：`变量的名称=值列表`。

* 变量的名称可以由大小写字母、阿拉伯数字和下划线构成，没有数据类型。

* 调用变量的时候可以用 "\$(VALUE_LIST)" 或者是 "${VALUE_LIST}" 来替换，这就是变量的引用

  * 例子：

    ```makefile
    OBJ=main.o test.o test1.o test2.o
    test:$(OBJ)
          gcc -o test $(OBJ)
    ```

* 四种基本赋值方式

  1.  简单赋值 ( := )

     * 编程语言中常规理解的赋值方式。

     * 例子：

       ```makefile
       x:=foo
       y:=$(x)b
       x:=new
       test：
             @echo "y=>$(y)"
             @echo "x=>$(x)"
       ```

     * 在 shell 命令行执行`make test`：

       ```shell
       y=>foob
       x=>new
       ```

  2. 递归赋值 ( = )

     * 例子：

       ```makefile
       x=foo
       y=$(x)b
       x=new
       test：
             @echo "y=>$(y)"
             @echo "x=>$(x)"
       ```

     * 在 shell 命令行执行`make test`：

       ```shell
       y=>newb
       x=>new
       ```

  3. 条件赋值 ( ?= )

     * 如果变量未定义，则使用符号中的值定义变量。如果该变量已定义，则该赋值语句无效。

     * 例子：

       ```makefile
       x:=foo
       y:=$(x)b
       x?=new
       test：
             @echo "y=>$(y)"
             @echo "x=>$(x)"
       ```

     * 在 shell 命令行执行`make test`：

       ```shell
       y=>foob
       x=>foo
       ```

  4.  追加赋值 ( += )

     * 原变量用空格隔开的方式追加一个新值。

     * 例子：

       ```makefile
       x:=foo
       y:=$(x)b
       x+=$(y)
       test：
             @echo "y=>$(y)"
             @echo "x=>$(x)"
       ```

     * 在 shell 命令行执行`make test`：

       ```shell
       y=>foob
       x=>foo foob
       ```

### 自动化变量

* 自动化变量为 Makefile 自动产生的变量，其值取决于执行规则的目标文件和依赖文件。

  | 自动化变量 | 说明                                                         |
  | ---------- | ------------------------------------------------------------ |
  | $@         | 表示规则的目标文件名，在多目标模式规则中，它代表的是触发规则被执行的文件名。 |
  | $%         | 当目标文件是一个静态库文件时，代表静态库的一个成员名。       |
  | $<         | 规则的第一个依赖的文件名。如果是一个目标文件使用隐含的规则来重建，则它代表由隐含规则加入的第一个依赖文件。 |
  | $?         | 所有比目标文件更新的依赖文件列表，空格分隔。                 |
  | $^         | 代表的是所有依赖文件列表，使用空格分隔。一个文件可重复的出现在目标的依赖中，变量“\$\^”只记录它的第一次引用的情况。 |
  | $+         | 类似“$^”，但是它保留了依赖文件中重复出现的文件。主要用于程序链接时库的交叉引用场合。 |
  | $*         | 在模式规则和静态模式规则中，代表“茎”（目标模式中“%”所代表的部分）。 |

* 例子：

  ```makefile
  test:test.o test1.o test2.o
           gcc -o $@ $^
  test.o:test.c test.h
           gcc -o $@ $<
  test1.o:test1.c test1.h
           gcc -o $@ $<
  test2.o:test2.c test2.h
           gcc -o $@ $<
  ```

  * "\$@" 代表目标文件test，"\$\^"代表依赖的文件，“$<”代表依赖文件中的第一个。
  * 在执行 make 的时候，会自动识别命令中的自动化变量，并自动实现自动化变量中值的替换，类似于编译C语言文件的时候的预处理。

* 例子：

  ```makefile
  lib:test.o test1.o test2.o
      ar r $?
  ```

  * 如果要做一个库文件，库文件的制作依赖于三个文件。当修改了其中的某个依赖文件，在命令行执行 make 命令，库文件 "lib" 就会自动更新。"$?" 表示修改的文件。

* GNU make 在这些变量中加入字符 "D" 或者 "F" ，形成了一系列变种的自动化变量，这些自动化变量可以对文件的名称进行操作。

  | 变量名          | 功能                                                         |
  | --------------- | ------------------------------------------------------------ |
  | $(@D)           | 表示文件的目录部分（不包括斜杠）。如果 "\$@" 表示的是 "dir/foo.o" 那么 "\$(@D)" 表示的值就是 "dir"。如果 "$@" 不存在斜杠（文件在当前目录下），其值就是 "."。 |
  | $(@F)           | 表示的是文件除目录外的部分（实际的文件名）。如果 "\$@" 表示的是 "dir/foo.o"，那么 "$@F" 表示的值为 "foo"。 |
  | \$\(\*D) \$(*F) | 分别代表 "茎" 中的目录部分和文件名部分                       |
  | \$(%D) $(%F)    | 当以 "archive(member)" 形式静态库为目标时，分别表示库文件成员 "member" 名中的目录部分和文件名部分。 |
  | \$(<D) $(<F)    | 表示第一个依赖文件的目录部分和文件名部分。                   |
  | \$(^D) $(^F)    | 表示所有依赖文件的目录部分和文件部分。                       |
  | \$(+D) $(+F)    | 表示所有依赖文件的目录部分和文件部分。                       |
  | \$(?D) $(?F)    | 表示更新的依赖文件的目录部分和文件名部分。                   |

### 目标文件搜索

* 如果需要的文件存在于不同的路径下，在编译时需要用到 Makefile 中目录搜索文件的功能。

* 常见的搜索方法的主要有两种：一般搜索VPATH和选择搜索vpath。

  * VPATH 是变量（环境变量），Makefile 中的一种特殊变量，使用时需要指定文件的路径。
  * vpath 是关键字，按照模式搜索，也可以说成是选择搜索。搜索的时候不仅需要加上文件的路径，还需要加上相应的限制条件。

* VPATH的使用：

  * 在 Makefile 中可以这样写：`VPATH := src`

  * 把 src 的值赋值给变量 VPATH，所以在执行 make 的时候会从 src 目录下找需要的文件。

  * 当存在多个路径时可以这样写：`VPATH := src car`或者`VPATH := src:car`，多个路径之间要使用空格或者是冒号隔开，表示在多个路径下搜索文件，搜索顺序为书写顺序。

  * 无论定义了多少路径，make 执行的时候会先搜索当前路径下的文件，当前目录下没有要找的文件，才去 VPATH 的路径中寻找。如果当前目录下有要使用的文件，那么 make 就会使用当前目录下的文件。

  * 例子：

    ```makefile
    VPATH=src car
    test:test.o
        gcc -o $@ $^
    ```

    * 假设 test.o 文件没有在当前的目录而在当前文件的子目录 "src" 或者是 "car" 下，程序执行是没有问题的，但是生成的 test 的文件是在当前的目录下（当然可以指定）。

* vpath的使用：

  * VPATH 是搜索路径下所有的文件，而 vpath 像是添加了限制条件，会过滤出一部分再去寻找。
  * 三种用法：
    1. ` vpath PATTERN DIRECTORIES`：
       * PATTERN：寻找的条件，DIRECTORIES：寻找的路径。
       * 例子：`vpath test.c src`
         * 在 src 路径下搜索文件 test.c。
       * 多路径的书写规则：`vpath test.c src car      或     vpath test.c src : car`
    2. `vpath PATTERN`：
       * 例子：`vpath test.c`
         * 清除符合文件 test.c 的搜索目录。
    3. `vpath`：
       * 清除所有已被设置的文件搜索路径。
  * 搜索的条件中可以包含模式字符“%”用于匹配一个或者是多个字符，例如“%.c”表示搜索路径下所有的 .c 结尾的文件。如果搜索条件中没有包含“%" ，那么搜索的文件就是具体的文件名称。

* 例子：

  * Makefile

  * src目录：list1.c、list2.c、main.c

  * include目录：list1.h、list2.h

  * 按照之前的方式编写 Makefile 文件：

    ```makefile
    main:main.o list1.o list2.o
        gcc -o $@ $<
    main.o:main.c
        gcc -o $@ $^
    list1.o:list1.c list1.h
        gcc -o $@ $<
    list2.o:list2.c list2.h
        gcc -o $@ $<
    ```

    * 错误：`make:*** No rule to make target 'main.c',need by 'main.o'. stop.`
    * 重建最终目标文件 main 的时候需要 main.o 文件，重建目标 main.o 文件的时候，没有找到指定的 main.c 文件，这是错误的根本原因。

  * 使用VPATH：

    ```makefile
    VPATH=src include
    main:main.o list1.o list2.o
        gcc -o $@ $<
    main.o:main.c
        gcc -o $@ $^
    list1.o:list1.c list1.h
        gcc -o $@ $<
    list2.o:list2.c list2.h
        gcc -o $@ $<
    ```

  * 使用vpath：

    ```makefile
    vpath %.c src
    vpath %.h include
    main:main.o list1.o list2.o
        gcc -o $@ $<
    main.o:main.c
        gcc -o $@ $^
    list1.o:list1.c list1.h
        gcc -o $@ $<
    list2.o:list2.c list2.h
        gcc -o $@ $<
    ```

### 隐含规则

* 可以使用隐含规则来简化 Makefile 文件编写。

* 例子：

  ```makefile
  test:test.o
      gcc -o test test.o
  test.o:test.c
  ```

  * 可以在 Makefile 中这样写来编译 test.c 源文件，相比较之前少了重建 test.o 的命令。但是执行 make，依然重建了 test 和 test.o 文件，运行结果却没有改变，这就是隐含规则的作用。在某些时候其实不需要给出重建目标文件的命令，有的甚至可以不需要给出规则：

  ```makefile
  test:test.o
      gcc -o test test.o
  ```

  * 隐含条件只能省略中间目标文件重建的命令和规则，但是最终目标的命令和规则不能省略。

* 隐含规则所提供的依赖文件只是一个基本的对应关系（在C语言中，通常是：test.o 对应 test.c 文件）。当需要增加这个文件的依赖文件的时候要在 Makefile 中使用没有命令行的规则给出：

  ```makefile
  test:test.o
      gcc -o test test.o
  test.o:test1.h
  ```

* 在有些时候隐含规则的使用会出现问题，因为有一个 make 的“隐含规则库”。库中的每一条隐含规则都有相应的优先级顺序，优先级高就会被优先使用。
  * 例如在 Makefile 中添加这行代码：`foo.o:foo.p`
  * .p 文件是 Pascal 程序的源文件，如果书写规则时不加入命令，那么 make 会按照隐含的规则来重建目标文件 foo.o。如果当前目录下恰好存在 foo.c 文件，隐含规则会把 foo.c 当做 foo.o 的依赖文件进行目标文件的重建。因为编译 .c 文件的隐含规则的优先级在编译 .p 文件之前。当 make 找到生成 foo.o 的文件之后，就不会再去寻找下一条规则。如果我们不想使用隐含规则，在使用的时候不仅要声明规则，也要添加上执行的命令。
* 内嵌隐含规则的命令中，所使用的变量都是预定义的，称这些变量为“隐含变量”，这些变量允许修改。
* 一些代表命令的常量：
  * AR：函数库打包程序，可创建静态库 .a 文档。
  * AS：应用于汇编程序。
  * CC：C 编译程序。
  * CXX：C++ 编译程序。
  * CO：从 RCS 中提取文件的程序。
  * CPP：C 程序的预处理器。
  * FC：编译器和与处理函数 Fortran 源文件的编译器。
  * GET：从 CSSC 中提取文件程序。
  * LEX：将 Lex 语言转变为 C 或 Ratfo 的程序。
  * PC：Pascal 语言编译器。
  * YACC：Yacc 文法分析器（针对于C语言）
  * YACCR：Yacc 文法分析器。

### 条件判断

* 条件语句：根据一个变量的值来控制 make 执行或者忽略 Makefile 的特定部分，条件语句可以是两个不同的变量或者是常量和变量之间的比较。
* 条件语句只能用于控制 make 实际执行的 Makefile 文件部分，不能控制规则的 shell 命令执行的过程。

| 关键字 | 功能                                              |
| ------ | ------------------------------------------------- |
| ifeq   | 判断参数是否不相等，相等为 true，不相等为 false。 |
| ifneq  | 判断参数是否不相等，不相等为 true，相等为 false。 |
| ifdef  | 判断是否有值，有值为 true，没有值为 false。       |
| ifndef | 判断是否有值，没有值为 true，有值为 false。       |

* ifeq和ifneq的使用方式：

  ```makefile
  ifeq (ARG1, ARG2)
  ifeq 'ARG1' 'ARG2'
  ifeq "ARG1" "ARG2"
  ifeq "ARG1" 'ARG2'
  ifeq 'ARG1' "ARG2"
  ```
  * 例子：

    ```makefile
    libs_for_gcc= -lgnu
    normal_libs=
    foo:$(objects)
    ifeq($(CC),gcc)
        $(CC) -o foo $(objects) $(libs_for_gcc)
    else
        $(CC) -o foo $(objects) $(noemal_libs)
    endif
    ```

  * 可以换一种更加简洁的方式：

    ```makefile
    libs_for_gcc= -lgnu
    normal_libs=
    ifeq($(CC),gcc)
        libs=$(libs_for_gcc)
    else
        libs=$(normal_libs)
    endif
    foo:$(objects)
        $(CC) -o foo $(objects) $(libs)
    ```

* ifdef和ifndef的使用方式：

  ```
  ifdef VARIABLE-NAME
  ```

  * 它的主要功能是判断变量的值是不是为空。

  * 例子：

    ```makefile
    bar =
    foo = $(bar)
    all:
    ifdef foo
        @echo yes		#执行，判断一个变量的值是否为空的时候需要使用"ifeq"而不是"ifdef"
    else
        @echo  no    
    endif
    ```

  * 例子：

    ```makefile
    foo=
    all:
    ifdef foo
        @echo yes
    else
        @echo  no		#执行
    endif
    ```

* 在条件表达式中不能使用自动化变量，自动化变量在规则命令执行时才有效。

### 伪目标

* 使用伪目标有两点原因：

  1. 避免 Makefile 中定义的只执行命令的目标和工作目录下的实际文件出现名字冲突。
  2. 提高执行 make 的效率。

* 当使用 make 命令行指定此目标时，这个目标所在规则定义的命令，无论目标文件是否存在都会被无条件执行。

* 情况一：需要书写一个规则，规则所定义的命令不是去创建文件，而是通过 make 命令明确指定它来执行一些特定的命令。

  ```makefile
  clean:
      rm -rf *.o test
  ```

  * rm 命令是执行删除任务，删除当前目录下的所有 .o 结尾和文件名为 test 的文件。当工作目录下不存在以 clean 为名的文件时，在 shell 中输入 make clean 命令，命令 rm -rf *.o test 就会被执行。
  * 如果当前目录下存在名为 clean 的文件，在 shell 中执行命令 make clean，由于这个规则没有依赖文件，所以目标被认为是最新的而不去执行规则所定义的命令，因此命令 rm 不会被执行。
  * 为了解决这个问题，删除 clean 文件或在 Makefile 中将目标 clean 声明为伪目标。将一个目标声明为伪目标的方法是将它作为特殊的目标`.PHONY`的依赖：`.PHONY:clean`。
  * 当一个目标被声明为伪目标之后，make 在执行此规则时不会试图查找隐含的关系去创建它，提高了 make 的执行效率。

  ```makefile
  .PHONY:clean
  clean:
      rm -rf *.o test
  ```

* 情况二：运用于 make 的并行和递归执行的过程中，此情况下一般会存在一个变量，定义为所有需要 make 的子目录。对多个目录进行 make 的实现，可以在一个规则的命令行中使用 shell 循环来完成。

  ```makefile
  SUBDIRS=foo bar baz
  subdirs:
      for dir in $(SUBDIRS);do $(MAKE) -C $$dir;done
  ```

  * 代码表达的意思是当前目录下存在三个子文件目录，每个子目录文件都有相对应的 Makefile 文件，代码中实现的部分是用当前目录下的 Makefile 控制其它子模块中的 Makefile 的运行，但是这种实现方法存在以下几个问题：

    1. 当子目录执行 make 出现错误时，make 不会退出，而是继续对其他的目录进行 make。在最终执行失败的情况下，根据错误提示很难定位出具体哪个目录下执行 make 发生错误。为了解决问题可以在命令部分加入错误检测，在命令执行错误后主动退出。然而如果在执行 make 时使用了 "-k" 选项，此方式将失效。
    2. 使用这种 shell 循环方式，没有用到 make 对目录的并行处理功能。由于规则的命令时一条完整的 shell 命令，不能被并行处理。

  * 使用伪目标：

    ```makefile
    SUBDIRS=foo bar baz
    .PHONY:subdirs $(SUBDIRS)
    subdirs:$(SUBDIRS)
    $(SUBDIRS):
        $(MAKE) -C $@
    foo:baz
    ```

    * 上面的实例中有一个没有命令行的规则“foo:baz”，这个规则用来规定三个子目录的编译顺序。因为在规则中 "baz" 的子目录被当作 "foo" 的依赖文件，所以 "baz" 要比 "foo" 子目录更先执行，最后执行 "bar" 子目录的编译。

* 一般情况下，一个伪目标不作为另外一个目标的依赖，因为当一个目标文件的依赖包含伪目标时，每一次在执行这个规则时，伪目标所定义的命令都会被执行。

* 当一个伪目标不是任何目标的依赖时，只能通过 make 命令来明确指定它的终极目标，执行它所定义的命令，例如make clean。

* 借助伪目标可以在一个文件里同时生成多个可执行文件：

  ```makefile
  .PHONY:all
  all:test1 test2 test3
  test1:test1.o
      gcc -o $@ $^
  test2:test2.o
      gcc -o $@ $^
  test3:test3.o
      gcc -o $@ $^
  ```

  * 在当前目录下创建三个源文件，把这三个源文件编译为三个可执行文件。将重建的规则放到 Makefile 中，约定使用伪目标 "all" 来作为最终目标，它的依赖文件就是要生成的可执行文件。这样的话只需要一个 make 命令，就会同时生成三个可执行文件。
  * 可以单独编译这三个中的任意一个源文件，如重建 test1，可以执行命令`make test1`。 

### 字符串处理函数

* 函数调用的格式：`$(<function> <arguments>)  或   ${<function> <arguments>}`

  * function 是函数名，arguments 是函数的参数，参数之间用逗号分隔。

* 模式字符串替换函数

  * 使用格式：`$(patsubst <pattern>,<replacement>,<text>)`

  * 查找 text 中的单词是否符合模式 pattern，如果符合的话，用 replacement 替换，返回值为替换后的新字符串。

  * 例子

    ```makefile
    OBJ=$(patsubst %.c,%.o,1.c 2.c 3.c)
    all:
        @echo $(OBJ)  #输出"1.o 2.o 3.o"
    ```

* 字符串替换函数

  * 使用格式：`$(subst <from>,<to>,<text>)`

  * 把字符串中的 form 替换成 to，返回值为替换后的新字符串。

  * 例子：

    ```makefile
    OBJ=$(subst ee,EE,feet on the street)
    all:
        @echo $(OBJ)  #输出“fEEt on the strEEt”
    ```

* 去空格函数

  * 使用格式：`$(strip <string>)`

  * 去掉字符串开头和结尾的空格，并且将其中多个连续的空格合并为一个空格，返回值为去掉空格后的字符串。

  * 例子：

    ```makefile
    OBJ=$(strip    a       b c)
    all:
        @echo $(OBJ)  #输出“a b c”
    ```

* 查找字符串函数

  * 使用格式：`$(findstring <find>,<in>)`

  * 查找 in 中的 find，如果查找的目标字符串存在，返回目标字符串，否则返回空。

  * 例子：

    ```makefile
    OBJ=$(findstring a,a b c)
    all:
        @echo $(OBJ)  #输出"a"
    ```

* 过滤函数

  * 使用格式：`$(filter <pattern>,<text>)`

  * 过滤出 text 中符合模式 pattern 的字符串，可以有多个 pattern，返回值为过滤后的字符串。

  * 例子：

    ```makefile
    OBJ=$(filter %.c %.o,1.c 2.o 3.s)
    all:
        @echo $(OBJ)  #输出“1.c 2.o”
    ```

* 反过滤函数

  * 使用格式：`$(filter-out <pattern>,<text>)`

  * 去除符合模式 pattern 的字符串，返回保留的字符串。

  * 例子：

    ```makefile
    OBJ=$(filter-out 1.c 2.o ,1.o 2.c 3.s)
    all：
        @echo $(OBJ)  #输出“3.s”
    ```

* 排序函数

  * 使用格式：`$(sort <list>)`

  * 将 list 中的单词升序，返回排序后的字符串。

  * 去除重复的字符串。

  * 例子：

    ```makefile
    OBJ=$(sort foo bar foo lost)
    all:
        @echo $(OBJ)  #输出“bar foo lost”
    ```

* 取单词函数

  * 使用格式：`$(word <n>,<text>)`

  * 取出 text 中的第 n 个单词，并返回。

  * 例子：

    ```makefile
    OBJ=$(word 2,1.c 2.c 3.c)
    all:
        @echo $(OBJ)  #输出“2.c”
    ```

### 文件名处理函数

* 取目录函数

  * 使用格式：`$(dir <names>)`

  * 从文件名序列 names 中取出目录部分，如果 names 中没有 "/" ，则取出 "./"，返回值为目录部分。

  * 例子：

    ```makefile
    OBJ=$(dir src/foo.c hacks)
    all:
        @echo $(OBJ)  #输出“src/ ./”
    ```

* 取文件函数

  * 使用格式：`$(notdir <names>)`

  * 从文件名序列 names 中取出非目录的部分。

  * 例子：

    ```makefile
    OBJ=$(notdir src/foo.c hacks)
    all:
        @echo $(OBJ)  #输出“foo.c hacks”
    ```

* 取后缀函数

  * 使用格式：`$(suffix <names>)`

  * 从文件名序列 names 中取出各个文件的后缀名，如果文件没有后缀名，则返回空字符串。

  * 例子：

    ```makefile
    OBJ=$(suffix src/foo.c hacks)
    all:
        @echo $(OBJ)  #输出“.c ”
    ```

* 取前缀函数

  * 使用格式：`$(basename <names>)`

  * 从文件名序列 names 中取出各个文件名的前缀部分(包含文件路径)，如果文件没有前缀名则返回空的字符串。

  * 例子：

    ```makefile
    OBJ=$(notdir src/foo.c hacks)
    all:
        @echo $(OBJ)  #输出“src/foo hacks”
    ```

*  添加后缀函数

  * 使用格式：`$(addsuffix <suffix>,<names>)`

  * 把后缀 suffix 加到 names 中的每个单词后面。

  * 例子：

    ```makefile
    OBJ=$(addsuffix .c,src/foo.c hacks)
    all:
        @echo $(OBJ)  #输出“sec/foo.c.c hack.c”
    ```

* 添加前缀函数

  * 使用格式：`$(addperfix <prefix>,<names>)`

  * 把前缀 prefix 加到 names 中的每个单词的前面。

  * 例子：

    ```makefile
    OBJ=$(addprefix src/, foo.c hacks)
    all:
        @echo $(OBJ)  #输出"src/foo.c src/hacks"
    ```

*  链接函数

  * 使用格式：`$(join <list1>,<list2>)`

  * 把 list2 中的单词对应的拼接到 list1 的后面，多出来的单词保持原样。

  * 例子：

    ```makefile
    OBJ=$(join src car,abc zxc qwe)
    all:
        @echo $(OBJ)  #输出“srcabc carzxc qwe”
    ```

*  获取匹配模式文件名函数

  * 使用格式：`$(wildcard PATTERN)`

  * 列出当前目录下所有符合模式 PATTERN 格式的文件名。

  * 例子：

    ```makefile
    OBJ=$(wildcard *.c  *.h)
    all:
        @echo $(OBJ)  #输出当前函数下所有以 ".c " 和  ".h"  为结尾的文件
    ```

### 其它常用函数

* `$(foreach <var>,<list>,<text>)`

  * 把参数`<list>`中的单词逐一取出放到参数`<var>`所指定的变量中，然后执行`<text>`所包含的表达式。每次`<text>`会返回一个字符串。最后整个循环结束的时候，`<text>`返回的每个字符串所组成的整个字符串（以空格分隔）将会是 foreach 函数的返回值。

  *  `<var>` 参数是一个临时的局部变量，作用域只在 foreach 函数中。

  * 例子：

    ```makefile
    name:=a b c d
    files:=$(foreach n,$(names),$(n).o)
    all:
        @echo $(files)  #输出“a.o b.o c.o d.o”
    ```

* `$(if <condition>,<then-part>) 或 (if<condition>,<then-part>,<else-part>)`

  * `condition`参数是 if 表达式，如果其返回的是非空字符串，那么这个表达式就相当于返回真，`then-part`就会被计算，否则`else-part`会被计算。

  * 如果`condition`为真，那么`then-part`是整个函数的返回值，否则`else-part`是返回值。如果`else-part`没有被定义，那么返回空字串符。

  * 例子：

    ```makefile
    OBJ:=foo.c
    OBJ:=$(if $(OBJ),$(OBJ),main.c)
    all:
          @echo $(OBJ)  #输出“foo.c”
    ```

* `$(call <expression>,<parm1>,<parm2>,<parm3>,...)`

  * `expression`参数中的变量\$(1)、\$(2)、$(3)等，会被参数`parm1`，`parm2`，`parm3`等依次取代，`expression`的返回值就是 call 函数的返回值。

  * 例子：

    ```makefile
    reverse = $(1) $(2)
    foo = $(call reverse,a,b)
    all：
          @echo $(foo)  #输出“a b”
    ```

  * 例子：

    ```makefile
    reverse = $(2) $(1)
    foo = $(call reverse,a,b)
    all:
          @echo $(foo)  #输出“b a”
    ```

* `$(origin <variable>)`

  * variable 是变量的名字，不应该是引用，最好不要在 variable 中使用“$”字符。

  * origin函数返回值：

    | 返回值       | 说明                                                         |
    | ------------ | ------------------------------------------------------------ |
    | undefined    | \<variable>没有定义过                                        |
    | default      | \<variable>是一个默认的定义，比如“CC”                        |
    | environment  | \<variable>是一个环境变量并且当Makefile被执行时，“-e”参数没有被打开 |
    | file         | \<variable>被定义在Makefile中                                |
    | command line | \<variable>这个变量是被命令执行的，将会被返回                |
    | override     | \<variable>是被override指示符重新定义的                      |
    | automatic    | \<variable>是命令运行中的自动化变量                          |

  * 例子：

    * 假设有一个 Makefile ，其包含了一个定义文件`Make.def`，其中定义了一个变量`bletch`，而环境变量中也有一个环境变量`bletch`，若想判断这个变量是不是环境变量，如果是就把它重定义了，于是在 Makefile 中，可以这样写：

      ```makefile
      ifdef bletch
      ifeq "$(origin bletch)" "environment"
      bletch = barf,gag,etc
      endif
      endif
      ```

### 命令编写

* make 处理 Makefile 过程中，如果没有明确指定，那么对规则中所有命令行的解析是使用`bin/sh`来完成的。

* 命令回显

  * 如果规则的命令行以字符“@”开始，则 make 在执行的时候不会显示这个将要被执行的命令，典型的用法是在使用`echo`命令输出一些信息时。

  * 例子：

    ```makefile
    OBJ=test main list
    all:
        @echo $(OBJ)
    ```

    * 执行时将会得到`test main list`这条输出信息，如果在执行命令之前没有字符“@”，那么make的输出将是`echo test main list`。

  * 当使用 make 的时候加上参数`-n`或`--just-print` ，则执行时只显示所要执行的命令，而不会真正的执行这个命令，对于调试 Makefile 非常有用。

  * make 参数`-s`或`--slient`则是禁止所有执行命令的显示，好像所有命令行都使用“@”开始一样。

* 命令执行

  * 当规则中的目标需要被重建时，此规则所定义的命令将会被执行，如果是多行的命令，那么每一行命令将在一个独立的子 shell 进程中执行，因此多命令行之间的命令执行是相互独立的。

  * Makefile 中书写在同一行的多个命令属于一个完整的 shell 命令，书写在独立行的命令是一个独立的 shell 命令。因此在一个规则的命令中，命令行 “cd” 改变目录不会对其后面的命令执行产生影响，即之后的命令执行的工作目录不会是之前使用“cd”进入的那个目录。如果要达到这个目的，应该把这两个命令放在一行上用分号隔开，这样才是一个完整的 shell 命令行。

  * 例子：

    ```makefile
    foo:bar/lose
        cd bar;gobble lose >../foo
    ```

  * 如果想把一个完整的shell命令行书写在多行，需要使用反斜杠 (\\) 来对处于多行的命令进行连接，表示它们是一个完整的shell命令：

    ```makefile
    foo:bar.lose
        cd bar; \
        gobble lose > ../foo
    ```

  * “SHELL”作为一个变量可以在 Makefile 中明确的给它赋值，其默认值为“/bin/sh”。

* 并发执行

  * 通常情况下，同一时刻只有一个命令在执行，下一个命令只有在当前命令结束之后才能执行。
  * 通过 make 命令行选项 "-j" 或 "--jobs" 可以允许多条命令同时执行。
  * 如果选项 "-j" 之后存在一个整数，其含义是允许同时执行的命令行的数目，默认为1。

### include文件包含

* 当 make 读取到 "include" 关键字的时候，会暂停读取当前的 Makefile，而是去读 "include" 包含的文件，读取结束后再继读取当前的 Makefile 文件。
* "include" 使用方式：`include <filenames>`
  * filenames 是 shell 支持的文件名（可以使用通配符表示的文件）。
  * include不能使用 Tab 开始，否则会把 "include" 当作命令来处理。
  * 包含的多个文件之间要使用空格分隔开。
* 使用场合：
  * 在一个工程文件中，每一个模块都有一个独立的 Makefile 来描述它的重建规则，它们需要定义一组通用的变量定义或者是模式规则。通常的做法是将这些共同使用的变量或者模式规则定义在一个文件中，需要的时候用 "include" 包含这个文件。
  * 当根据源文件自动产生依赖文件时，可以将自动产生的依赖关系保存在另一个文件中，然后在 Makefile 中包含这个文件。
* 在执行 make 命令的时候可以加入选项 "-I" 或 "--include-dir" ，后面添加指定的路径，如果文件存在就会被使用，否则就会在其他的几个路径中搜索："usr/gnu/include"、"usr/local/include" 和 "usr/include"。
* 如果没有找到 "include" 指定的文件，make 将会有警示，但不会退出，而是继续执行 Makefile 的后续内容。当完成读取整个 Makefile 后，make 将试图使用规则来创建通过 "include" 指定但不存在的文件，不能创建时文件将会保存退出。
* 用 "-include" 代替 "include" 来忽略文件不存在或无法创建的错误提示，使用格式：`-include <filename>`
  * 使用 `include <filenames>` ，make 在处理程序的时候，文件列表中的任意一个文件不存在或没有规则去创建这个文件，make 程序将会提示错误并保存退出。
  * 使用 `-include <filenames>`，当包含的文件不存在或没有规则去创建它的时候，make 将会继续执行程序，只有不能完成终极目标重建的时候才会提示错误并保存退出。

### 嵌套执行make

* 例子：

  ```makefile
  subsystem:
      cd subdir && $(MAKE)
  ```

  * 在当前目录下有一个目录文件 subdir 和一个 Makefile 文件，子目录 subdir 文件下还有一个 Makefile 文件，这个文件用来描述这个子目录文件的编译规则。

  * 使用时只需在最外层的目录中执行 make 命令，当命令执行到上述规则时，程序会进入到子目录中执行 make。

  * 另一种写法：

    ```makefile
    subsystem
        $(MAKE) -C subdir
    ```

  * 在 make 的嵌套执行中，变量 "CURDIR" 代表 make 的工作目录。当使用 make 的选项 "-C" 的时候，命令就会进入指定的目录中，然后此变量会被重新赋值。如果在 Makefile 中没有对此变量进行显式的赋值，那么它就表示 make 的工作目录。

* 嵌套执行时如果需要变量的传递，可以`export <variable>`，如果不需要则`unexport <variable>`。

  * \<variable>是变量的名字，不需要使用 "$"，如果所有的变量都需要传递，只需要使用 "export"。

* Makefile 中的变量 SHELL 和 MAKEFLAGS，总会传递到下层的 Makefile 中。MAKEFLAGS 变量包含了 make 的参数信息。

  * 以下几个参数选项并不传递："-C"、"-f"、"-o"、"-h" 和 "-W"。

  * 如果不传递 MAKEFLAGS 变量的值，可以这样写：

    ```makefile
    subsystem:
        cd subdir && $(MAKE) MAKEFLAGS=
    ```

* 例子：

  ```
  ├──Makefile         //最外层的Makefile文件，不是目录文件。
  ├──include          //编译的时候需要链接的库文件
  │      ├──codec   //libui.a 库文件所在的目录
  │      ├──db        //libdb.a 库文件所在的目录
  │      ├──ui         //libui.a库文件所在的目录
  ├──lib                   //源文件所在的目录，子目录文件中包含Makefile文件
  │      ├──codec     //编解码器所在的源文件的目录
  │      ├──db           //数据库源文件所在的目录
  │      ├──ui            //用户界面源文件所在目录
  ├──app
  │      ├──player    
  └──doc              //这个工程编译说明    
  ```

  * 假设只有 lib 目录和 app 目录下的各个子目录含有 Makefile 文件，则总控的 Makefile 文件可以这样来写：

    ```makefile
    lib_codec := lib/codec
    lib_db    := lib/db
    lib_ui     := lib/ui
    libraries   := $(lib_codec) $(lib_db) $(lib_ui)
    player    := app/player
    .PHONY : all $(player) $(libraries)
    all : $(player)
    $(player) $(libraries) :
        $(MAKE) -C $@
    ```

  * 当 make 在建立依存图的时候找不到程序库与 app/player 工作目标之间的依存关系，意味着建立任何程序库之前，make 将先执行 app/player 目录中的 Makefile，这会导致失败的结果，因为应用程序的链接需要程序库。为解决这个问题，需提供额外的依存信息：

    ```makefile
    $(player) : $(libraries)
    $(lib_ui) : $(lib_db) $(lib_codec)
    ```

  * 在最外层执行 make 的时候会看到如下输出信息：

    ```shell
    make -C lib/db
    make[1]: Entering directory ‘/MP3_player/lib/db’
    make[1]:Update db library...
    make[1]: Leaving directory ‘/MP3_player/lib/db’
    make -C lib/codec
    make[1]: Entering directory ‘/MP3_player/lib/codec’
    make[1]:Update codec library...
    make[1]: Leaving directory ‘/MP3_player/lib/codec’
    make -C lib/ui
    make[1]: Entering directory ‘/MP3_player/lib/ui’
    make[1]:Update ui library...
    make[1]: Leaving directory ‘/MP3_player/lib/ui’
    make -C app/player
    make[1]: Entering directory ‘/MP3_player/app/player’
    make[1]:Update player library...
    make[1]: Leaving directory ‘/MP3_player/app/player’
    ```

  * 当 make 发觉它正在递归调用另一个 make 时，会启用--print-directory(-w) 选项，使得 make 输出 Entering directory(进入目录) 和 Leaving directory(离开目录) 的信息。

### make命令参数和选项

| 参数选项                                                     | 功能                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| -b，-m                                                       | 忽略，提供其他版本 make 的兼容性                             |
| -B，--always-make                                            | 强制重建所有的规则目标，不根据规则的依赖描述决定是否重建目标文件 |
| -C DIR，--directory=DIR                                      | 在读取 Makefile 之前，进入到目录 DIR，然后执行 make。当存在多个 "-C" 选项的时候，make 的最终工作目录是第一个目录的相对路径 |
| -d                                                           | make 在执行的过程中打印出所有调试信息，包括 make 认为哪些文件需要重建，哪些文件需要比较最后的修改时间、比较的结果，重建目标是用的命令还是规则等等。使用 "-d" 选项可以显示 make 构造依赖关系链、重建目标过程中的所有信息。 |
| --debug[=OPTIONS]                                            | make 执行时输出调试信息，可以使用 "OPTIONS" 控制调试信息的级别。默认是 "OPTIONS=b" ，"OPTIONS" 的值可为：all、basic、verbose、implicit、jobs、makefile（首字母有效）。 |
| -e，--enveronment -overrides                                 | 使用环境变量定义覆盖 Makefile 中的同名变量定义               |
| -f=FILE，--file=FILE， --makefile=FILE                       | 指定文件 "FILE" 为 make 执行的 Makefile 文件                 |
| -p，--help                                                   | 打印帮助信息                                                 |
| -i，--ignore-errors                                          | 执行过程中忽略规则命令执行的错误                             |
| -I DIR，--include-dir=DIR                                    | 指定搜索目录，在Makefile中出现另一个 "include" 文件时，将在 "DIR" 目录下搜索。多个 "-i" 指定目录时，搜索目录按照指定的顺序进行 |
| -j [JOBS]，--jobs[=JOBS]                                     | 可指定同时执行的命令数目，没有 "-j" 的情况下，执行的命令数目将是系统允许的最大可能数目，存在多个 "-j" 目标时，最后一个目标指定的 JOBS 数有效 |
| -k，--keep-going                                             | 执行命令错误时不终止 make 的执行，make 尽最大可能执行所有的命令 |
| -l load，--load-average=[=LOAD]，--max-load[=LOAD]           | 告诉 make 当存在其他任务执行的时候，如果系统负荷超过 "LOAD"，不再启动新的任务，如果没有指定 "LOAD" 的参数 ，"-l" 选项将取消之前 "-l" 指定的限制 |
| -n，--just-print，--dry-run                                  | 只打印执行的命令，但是不执行命令                             |
| -o FILE，--old-file=FILE， --assume-old=FILE                 | 指定 "FILE" 文件不需要重建，即使它的依赖已经过期，同时不重建此依赖文件的任何目标，此参数不会通过变量 "MAKEFLAGS" 传递给子目录进程 |
| -p，--print-date-base                                        | 命令执行之前，打印出 make 读取的 Makefile 的所有数据，同时打印出 make 的版本信息。如果只需要打印这些数据信息，可以使用 "make -qp" 命令，查看 make 执行之前预设的规则和变量，可使用命令 "make -p -f /dev/null" |
| -q，-question                                                | "询问模式"，不运行任何的命令，并且无输出。make 只返回一个查询状态，返回状态 0 表示没有目标需要重建，返回状态 1 表示存在需要重建的目标，返回状态 2 表示有错误发生 |
| -r，--no-builtin-rules                                       | 取消所有内嵌函数的规则，不过可以在 Makefile 中使用模式规则来定义规则。同时选项 "-r" 会取消所有后缀规则的隐含后缀列表，同样可以在 Makefile 中使用 ".SUFFIXES"定义后缀名的规则。"-r" 选项不会取消 make 内嵌的隐含变量 |
| -R，--no-builtin-variabes                                    | 取消 make 内嵌的隐含变量，不过可以在 Makefile 中明确定义某些变量。"-R" 和 "-r" 选项同时打开，因为没有了隐含变量，所以隐含规则将失去意义 |
| -s，--silent，--quiet                                        | 取消命令执行过程中的打印                                     |
| -S，--no-keep-going， --stop                                 | 可以在子 make 中使用 “-S” 选项取消上层传递的 "-k" 选项，或者取消系统环境变量 "MAKEFLAGS" 中 "-k" 选项 |
| -t，--touch                                                  | 更新所有目标文件的时间戳到当前系统时间，防止 make 对所有过时目标文件的重建 |
| -v，version                                                  | 查看make的版本信息                                           |
| -w，--print-directory                                        | 在 make 进入一个子目录读取 Makefile 之前打印工作目录，这个选项可以帮助调试 Makefile，跟踪定位错误。使用 "-C" 选项时默认打开这个选项 |
| --no-print-directory                                         | 取消 "-w" 选项，可以用在递归的 make 调用过程中 ，取消 "-C" 参数默认打开 "-w" 的功能 |
| -W FILE，--what-if=FILE， --new-file=FILE， --assume-file=FILE | 设定文件 "FILE" 的时间戳为当前的时间，但不更改文件实际的最后修改时间。此选项主要是为了实现对所有依赖于文件 "FILE" 的目标的强制重建 |
| --warn-undefined-variables                                   | 在发现 Makefile 中存在没有定义的变量被引用时给出告警信息，此功能可以帮助调试一个存在多级嵌套变量引用的 Makefile |

### 目标类型

* 强制目标

  * 如果一个规则中没有命令或依赖，并且它的目标不是一个存在的文件名，在执行此规则时，目标总会被认为是最新的。这样的目标在作为一个规则的依赖时，因为依赖总被认为更新过，因此这个规则中定义的命令总会被执行。

  * 例子：

    ```makefile
    clean:FORCE
        rm $(OBJECTS)
    FORCE:
    ```

    * 此例中使用 "FORCE" 目标的效果和将 "clean" 声明为伪目标的效果相同。

* 空目标文件

  * 伪目标的一个变种，和伪目标不同的是这个目标可以是一个存在的文件，但并不关心文件的具体内容，通常此文件是一个空文件。

  * 用来记录上一次执行此规则的命令的时间，在这样的规则中，命令部分都会使用 "touch" 在完成所有的命令之后来更新目标文件的时间戳，记录此规则命令的最后执行时间。make 时通过命令行将此目标作为终极目标，当前目标下如果不存在这个文件，"touch" 会在第一次执行时创建该文件。

  * 一个空目标文件应该存在一个或多个依赖文件。将这个目标作为终极目标，在它所依赖的文件比它更新时，此目标所在的规则的命令行将被执行。

  * 例子：

    ```makefile
    print:foot.c bar.c
        lpr -p $?
        touch print
    ```

* 特殊目标

  | 名称                  | 功能                                                         |
  | --------------------- | ------------------------------------------------------------ |
  | .PHONY                | 这个目标的所有依赖为伪目标。伪目标：当使用 make 命令行指定此目标时，这个目标所在规则定义的命令，无论目标文件是否存在都会被无条件执行 |
  | .SUFFIXES             | 这个目标的所有依赖指出了一系列在后缀规则中需要检查的后缀名   |
  | .DEFAULT              | 这个目标所在规则定义的命令，被用于重建那些没有具体规则的目标，就是说一个文件作为某个规则的依赖，却不是另外一个规则的目标时，make 程序无法找到重建此文件的规则，这种情况就执行 ".DEFAULT" 所指定的命令 |
  | .PRECIOUS             | 这个目标的依赖文件在 make 的过程中会被特殊处理，当命令执行过程中断时，make 不会删除它们。如果目标的依赖文件是中间过程文件，这些文件同样不会被删除 |
  | .INTERMEDIATE         | 这个目标的依赖文件在 make 执行时被作为中间文件对待，没有任何依赖文件的这个目标没有意义 |
  | .SECONDARY:           | 这个目标的依赖文件被视为中间文件，但是这些文件不会被删除。这个目标没有任何依赖文件的含义是将所有的文件视为中间文件 |
  | .IGNORE               | 这个目标的依赖文件忽略创建这个文件所执行命令的错误，给此目标指定命令是没有意义的。当此目标没有依赖文件时，忽略所有命令执行的错误 |
  | .DELETE_ON_ERROR      | 如果存在目标 ".DELETE_ON_ERROR" ，make 在执行过程中，若规则的命令执行错误，将删除已经被修改的目标文件 |
  | .LOW_RESOLUTION_TIME  | 这个目标的依赖文件被 make 认为是低分辨率时间戳文件，给这个目标指定命令是没有意义的。通常的目标都是高分辨率时间戳文件 |
  | .SILENT               | make 在创建这个目标的依赖文件时，不打印此文件所执行的命令。同样，给目标 "SILENT" 指定命令行是没有意义的 |
  | .EXPORT_ALL_VARIABLES | 此目标作为一个简单的没有依赖的目标，它的功能是将之后的所有变量传递给子 make 进程 |
  | .NOTPARALLEL          | 如果出现这个目标，则所有的命令按照串行的方式执行，即使存在 make 的命令行参数 "-j" 。但在递归调用的子make进程中，命令行可以并行执行。此目标不应该有依赖文件，所有出现的依赖文件将会被忽略 |

* 多规则目标

  * 对于一个多规则的目标，重建这个目标的命令只能出现在一个规则中。如果多个规则同时给出重建此目标的命令，make 将使用最后一个规则中所定义的命令，同时给出错误信息。

  * 一个仅仅描述依赖关系的规则可以用来给出一个或者多个目标文件的依赖文件。例如，Makefile 中通常存在一个变量，它定义为所有需要编译生成的 .o 文件列表。这些 .o 文件在其源文件中包含的头文件 "config.h" 发生变化之后能够自动被重建，可以使用多目标的方式来书写 Makefile：

    ```makefile
    objects=foo.o bar.o
    foo.o:defs.h
    bar.o:defs.h test.h
    $(objects):config.h
    ```

  * 可以通过一个变量来增加目标的依赖文件，使用 make 的命令行来指定某一个目标的依赖头文件：

    ```makefile
    extradeps=
    $(objects):$(exteradeps)
    ```

    * 如果执行 "make exteradeps=foo.h"，那么 "foo.h" 将作为所有的 .o 文件的依赖文件，如果只执行 "make" 的话，就没有指定任何文件作为 .o 文件的依赖文件。

### 变量的高级用法

* 替换引用

  * 例子：

    ```makefile
    foo:=a.c b.c d.c
    obj:=$(foo:.c=.o)
    All:
        @echo $(obj)  #输出"a.o b.o d.o"
    ```

    * 这段代码实现的功能是字符串后缀名的替换，把变量 foo 中以 .c 结尾的字符串全部替换成 .o 结尾的字符串。
    * 括号中的变量使用的是变量名而不是变量名的引用，变量名的后面要使用冒号和参数选项分开，表达式中间不能使用空格，第二个变量 obj 是对整体的引用。

  * 更加通用的书写方式：

    ```makefile
    foo:=a.c b.c d.c
    obj:=$(foo:%.c=%.o)
    All:
        @echo $(obj)
    ```

    * 这种方式比第一种方式更加实用，因为在实际使用过程中，对变量值的操作可能不只修改其中的一个部分：

      ```makefile
      foo:=a123c a1234c a12345c
      obj:=$(foo:a%c=x%y)
      All:
          @echo $(obj)  #输出"x123y x1234y x12345y"
      ```

* 嵌套使用

  * 可以在一个变量的赋值中引用其它的变量，并且引用变量的数量和次数没有限制。

  * 例子：

    ```makefile
    foo:=test
    var:=$(foo)
    All:
        @echo $(var)  #输出“test”
    ```

  * 例子：

    ```makefile
    foo=var
    var=test
    var:=$($(foo))
    All:
        @echo $(var)  #输出“test”
    ```

  * 使用变量的时候，并不是只能引用一个变量，可以有多个变量的引用，还可以包含一些文本字符。

  * 例子：

    ```makefile
    first_pass=hello
    bar=first
    var:=$(bar)_pass
    all:
        @echo $(var)  #输出“hello”
    ```

  * 例子：

    ```makefile
    first_pass=hello
    bar=first
    foo=pass
    var:=$(bar)_$(foo)
    all:
        @echo $(var)  #输出“hello”
    ```

* 在实际使用中变量的第一种用法较常用，第二种用法很少使用，应该说是尽量避免使用变量的嵌套引用，必须要使用的时候做到嵌套的层数尽可能少。

### 控制函数error和warning

* `$(error TEXT...)`

  * 产生致命错误，并提示 "TEXT..." 信息给用户，并退出 make 的执行，返回空。

  * "error" 函数是在函数被调用时才提示信息并结束 make 进程，因此如果函数出现在命令中或者一个递归的变量定义时，读取 Makefile 时不会出现错误。而只有包含 "error" 函数引用的命令被执行，或者定义中引用此函数的递归变量被展开时，才会提示信息 "TEXT..." 同时退出 make。

  * "error" 函数一般不出现在直接展开式的变量定义中，否则在 make 读取 Makefile 时将会提示致命错误。

  * 例子：

    ```makefile
    ERROR1=1234
    all:
        ifdef ERROR1
        $(error error is $(ERROR1))
        endif     
    ```

    * make 读取 Makefile 时，如果所起的变量名是已经定义好的"ERROR1"，make 将会提示错误信息 "error is 1234" 并保存退出。

  * 例子：

    ```makefile
    ERR=$(error found an error!)
    .PHONY:err
    err:;$(ERR)
    ```

    * make 读取 Makefile 时不会出现错误，只有 "err" 作为一个目标被执行时才会出现错误。

* `$(warning TEXT...)`

  * 不会导致致命错误（make不退出），只是提示 "TEXT..."，make 的执行过程继续，返回空。

### 常见错误信息

* make 执行过程中产生的错误并不都是致命的，特别是在命令行之前存在 "-" 或 make 使用 "-k" 选项执行时。

* make 执行过程的致命错误都带有前缀字符串 "***"。错误信息都有前缀，一种是执行程序名作为错误前缀（通常是 "make"）；另外一种是当 Makefile 本身存在语法错误无法被 make 解析并执行时，前缀包含了 Makefile 文件名和出现错误的行号。

* ```shell
  [FOO] Error NN
  [FOO] signal description
  ```

  * 省略了普通前缀，并不是 make 的真正错误。它表示 make 检测到 make 所调用的作为执行命令的程序返回一个非零状态（Error NN），或者此命令程序以非正常方式退出（携带某种信号）。
  * 如果错误信息中没有附加 "***" 字符串，则是子过程的调用失败，如果 Makefile 中此命令有前缀 "-"，make 会忽略这个错误。

* ```shell
  missing separator. Stop.
  missing separator (did you mean TAB instead of 8 spaces?). Stop.
  ```

  * 不可识别的命令行，make 在读取 Makefile 过程中不能解析其中包含的内容。GNU make在读取 Makefile 时根据各种分隔符（:, =, [TAB]字符等）来识别 Makefile 的每一行内容。这些错误意味着 make 不能发现一个合法的分隔符。

* ```shell
  commands commence before first target. Stop.
  missing rule before commands. Stop.
  ```

  * Makefile 可能以命令行开始，即以 [Tab] 字符开始，但不是一个合法的命令行，命令行必须和规则一一对应。
  * 产生第二种错误的原因可能是一行的第一个非空字符为分号，make 会认为此处遗漏了规则的 "target: prerequisite" 部分。

* ```shell
  No rule to make target 'XXX'.
  No rule to make target 'XXX ', needed by 'yyy'.
  ```

  * 无法为重建目标“XXX”找到合适的规则，包括明确规则和隐含规则。
  * 修正这个错误的方法是在 Makefile 中添加一个重建目标的规则。其它可能导致这些错误的原因是 Makefile 中文件名拼写错误，或者破坏了源文件树。

* ```shell
  No targets specified and no makefile found. Stop.
  No targets. Stop.
  ```

  * 第一个错误表示在命令行中没有指定需要重建的目标，并且 make 不能读入任何 Makefile 文件。
  * 第二个错误表示能够找到 Makefile 文件，但没有终极目标或者没有在命令行中指出需要重建的目标。这种情况下，make 什么也不做。

* ```shell
  Makefile 'XXX' was not found.
  Included makefile 'XXX' was not found.
  ```

  * 未使用 "-f" 指定 Makefile 文件，make 不能在当前目录下找到 Makefile（makefile 或GNUmakefile）。
  * 使用 "-f" 指定文件，但不能读取这个指定的 Makefile 文件。

* ```shell
  warning: overriding commands for target 'XXX'
  warning: ignoring old commands for target 'XXX'
  ```

  * 对同一目标 "XXX" 存在一个以上的重建命令。
  * GNU make 规定：当同一个文件作为多个规则的目标时，只能有一个规则定义重建它的命令（双冒号规则除外）。如果为一个目标多次指定了相同或者不同的命令，就会产生第一个告警。
  * 第二个告警信息指出新指定的命令覆盖了上一次指定的命令。

* ```shell
  Circular XXX <- YYY dependency dropped.
  ```

  * 规则的依赖产生了循环，目标 "XXX" 的依赖文件为 "YYY"，而依赖 "YYY" 的依赖列表中包含 "XXX"。

* ```shell
  Recursive variable 'XXX' references itself (eventually). Stop.
  ```

  * make 的变量 "XXX"（递归展开式）在替换展开时，引用它自身。无论对于直接展开式变量（通过:=定义的）或追加定义（+=），这都是不允许的。

* ```shell
  Unterminated variable reference. Stop.
  ```

  * 变量或者函数引用语法不正确，没有使用完整的的括号（缺少左括号或者右括号）。

* ```shell
  insufficient arguments to function 'XXX'. Stop.
  ```

  * 引用函数 "XXX" 时参数数目不正确，函数缺少参数。

* ```shell
  missing target pattern. Stop.
  multiple target patterns. Stop.
  target pattern contains no '%'. Stop.             
  mixed implicit and static pattern rules. Stop.
  ```

  * 不正确的静态模式规则。
  * 第一条错误的原因：静态模式规则的目标段中没有模式目标。
  * 第二条错误的原因：静态模式规则的目标段中存在多个模式目标。
  * 第三条错误的原因：静态模式规则的目标段目标模式中没有包含模式字符“%”。
  * 第四条错误的原因：静态模式规则的三部分都包含了模式字符“%”，正确的应该是只有后两个才可以包含模式字符“%”。

* ```shell
  warning: -jN forced in submake: disabling jobserver mode.
  ```

  * make 检测到递归的 make 调用时，可通信的子 make 进程出现并行处理的错误。递归执行的 make 的命令行参数中存在 "-jN" 参数（N的值大于1），在有些情况下可能导致此错误，例如：Makefile 中变量 "MAKE" 被赋值为 "make –j2"，并且递归调用的命令行中使用变量 "MAKE"。在这种情况下，被调用 make 进程不能和其它 make 进程进行通信，其只能简单的独立的并行处理两个任务。

* ```shell
  warning: jobserver unavailable: using -j1. Add '+' to parent make rule.
  ```

  * 为了实现 make 进程之间的通信，上层 make 进程将传递信息给子 make 进程。在传递信息过程中可能存在这种情况，子 make 进程不是一个实际的 make 进程，而上层 make 却不能确定子进程是否是真实的 make 进程，它只是将所有信息传递下去。上层 make 采用正常的算法来决定这些。当出现这种情况，子进程只会接受父进程传递的部分有用的信息。子进程会产生该警告信息，之后按照其内建的顺序方式进行处理。