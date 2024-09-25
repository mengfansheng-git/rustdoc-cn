# Command-line arguments

这里是可以传递给 `rustdoc` 的参数列表：

## `-h`/`--help`: help

这样使用：

```bash
$ rustdoc -h
$ rustdoc --help
```

这会展示 `rustdoc` 内置的帮助，包含了大量可用的命令行 flags。

有些 flags 是未稳定的；这个页面只会只包好稳定的参数，`--help` 会包含所有的。

## `-V`/`--version`: version information

使用该flag的方式如下：

```bash
$ rustdoc -V
$ rustdoc --version
```

这将显示 `rustdoc` 的版本，如下所示：

```text
rustdoc 1.17.0 (56124baa9 2017-04-24)
```

## `-v`/`--verbose`: more verbose output

使用该flag的方式如下：

```bash
$ rustdoc -v src/lib.rs
$ rustdoc --verbose src/lib.rs
```

这将启用 "冗长模式"，即会将更多信息写入标准输出。
写入的内容取决于你传入的其它标志。
例如，使用 `--version`：

```text
$ rustdoc --verbose --version
rustdoc 1.17.0 (56124baa9 2017-04-24)
binary: rustdoc
commit-hash: hash
commit-date: date
host: host-triple
release: 1.17.0
LLVM version: 3.9
```

## `-o`/`--out-dir`: output directory path

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs -o target/doc
$ rustdoc src/lib.rs --out-dir target/doc
```

默认情况下，`rustdoc` 的输出会出现在当前工作目录下名为 `doc` 的目录中。
使用此标记后，它将把所有输出到你指定的目录。

## `--crate-name`: controlling the name of the crate

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --crate-name mycrate
```

默认情况下，"rustdoc"会假定你的 crate 名称与".rs "文件相同。
您可以使用 `--crate-name` 来覆盖这一假设。

## `--document-private-items`: Show items that are not public

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --document-private-items
```

默认情况下，`rustdoc` 只记录可公开访问的项目。

```rust
pub fn public() {} // this item is public and will be documented
mod private { // this item is private and will not be documented
    pub fn unreachable() {} // this item is public, but unreachable, so it will not be documented
}
```

`--document-private-items` documents all items, even if they're not public.

## `-L`/`--library-path`: where to look for dependencies

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs -L target/debug/deps
$ rustdoc src/lib.rs --library-path target/debug/deps
```

如果你的 crate 有依赖库，`rustdoc` 需要知道在哪里可以找到它们。
传递 `--library-path` 会给 `rustdoc` 提供一个查找这些依赖项的列表。

这个标志可以接受任意数量的目录作为参数，并在搜索时使用所有的目录。

## `--cfg`: passing configuration flags

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --cfg feature="foo"
```

该flag接受与 `rustc --cfg` 相同的值，并用它来配置编译。
上面的例子使用了 `feature`，但任何 `cfg` 值都可以接受。

## `--extern`: specify a dependency's location

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --extern lazy-static=/path/to/lazy-static
```

与"--library-path "类似，"--extern "是关于指定的位置。
library-path "提供了搜索目录，而"--extern则可以让你精
确地指定依赖项的位置。

## `-C`/`--codegen`: pass codegen options to rustc

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs -C target_feature=+avx
$ rustdoc src/lib.rs --codegen target_feature=+avx

$ rustdoc --test src/lib.rs -C target_feature=+avx
$ rustdoc --test src/lib.rs --codegen target_feature=+avx

$ rustdoc --test README.md -C target_feature=+avx
$ rustdoc --test README.md --codegen target_feature=+avx
```

当 rustdoc 生成文档、查找文档测试或执行文档测试时，它需要编译一些 rust 代码，至少是部分编译。
测试时，需要编译一些 rust 代码，至少是部分编译。这个flag允许你告诉 rustdoc
在运行这些编译时向 rustc 提供一些额外的 codegen 选项。
大多数情况下这些选项不会影响正常的文档运行，但如果某些内容依赖于目标特性
或者文档测试需要使用一些额外的选项，这个flag允许你影响。

该标志的参数与 rustc 的 `-C` 标志相同。运行 `rustc -C help` 以
获得完整的列表。

## `--test`: run code examples as tests

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --test
```

该flag将把你的代码示例作为测试运行。更多信息，请参阅(document-tests.md)。

另请参阅 `--test-args`。

## `--test-args`: pass options to test runner

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --test --test-args ignored
```

运行文档测试时，该flag将向测试运行器传递选项。
更多信息，请参阅(document-tests.md)。

另请参阅 `--test`。

## `--target`: generate documentation for the specified target triple

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --target x86_64-pc-windows-gnu
```

与 `rustc` 的 `--target` 标志类似，它会为与主机三元组不同的目标三元组生成文档。

交叉编译代码的所有常见注意事项均适用。

## `--default-theme`: set the default theme

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --default-theme=ayu
```

设置默认主题（适用于浏览器未从页面主题选择器中记住
的用户）。

提供的值应该是主题名称的小写版本。
可用主题集可在主题选择器中查看
生成的输出中的主题选择器中查看。

需要注意的是，可用主题集及其外观在不同版本的 rustdoc 中不一定稳定。
在不同的 rustdoc 版本之间并不一定稳定。如果
请求的主题不存在，则会使用内置的默认主题 (当前为
`light`) 会被使用。

## `--markdown-css`: include more CSS files when rendering markdown

使用该flag的方式如下：

```bash
$ rustdoc README.md --markdown-css foo.css
```

在渲染 Markdown 文件时，这将在生成的 HTML 的元素。例如，上面的调用

```html
<link rel="stylesheet" type="text/css" href="foo.css" />
```

将被添加。

在渲染 Rust 文件时，该标记将被忽略。

## `--html-in-header`: include more HTML in <head>

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --html-in-header header.html
$ rustdoc README.md --html-in-header header.html
```

该标记接收文件列表，并将其插入渲染文档的 `<head>` 部分。

## `--html-before-content`: include more HTML before the content

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --html-before-content extra.html
$ rustdoc README.md --html-before-content extra.html
```

该标记会获取一个文件列表，并将它们插入`<body>`标记内，但在`rustdoc`通常会在渲染时生成的其他内容之前
之前插入。

## `--html-after-content`: include more HTML after the content

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --html-after-content extra.html
$ rustdoc README.md --html-after-content extra.html
```
这个标志会读取一个文件列表，并将它们插入到生成的文档中，位置在`</body>`标签之前，
但在通常由`rustdoc`生成的其他内容之后。


## `--markdown-playground-url`: control the location of the playground

使用该flag的方式如下：

```bash
$ rustdoc README.md --markdown-playground-url https://play.rust-lang.org/
```
当渲染Markdown文件时，此标志会提供Rust Playground的基URL，用于生成`Run`按钮。

## `--markdown-no-toc`: don't generate a table of contents

使用该flag的方式如下：

```bash
$ rustdoc README.md --markdown-no-toc
```
当从 Markdown 文件生成文档时，默认情况下，`rustdoc` 会生成一个目录。此标志会禁用此功能，不会生成目录。

## `-e`/`--extend-css`: extend rustdoc's CSS

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs -e extra.css
$ rustdoc src/lib.rs --extend-css extra.css
```

使用此标志，您传递的文件的内容将包含在底部Rustdoc的`theme.css`文件。

虽然这个标志是稳定的，但`theme.css`的内容不是，所以要小心!
更新可能会破坏您的主题扩展。


## `--sysroot`: override the system root

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --sysroot /path/to/sysroot
```

与`rustc --sysroot`类似，这允许您更改`rustdoc`使用的sysroot在编译代码时。

### `--edition`: control the edition of docs and doctests

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --edition 2018
$ rustdoc --test src/lib.rs --edition 2018
```

这个标志允许`rustdoc`将您的rust代码视为给定的版本。它将编译doctest
给定的版本也是如此。和 `rustc`一样，`rustdoc`使用的默认版本是`2015`
(第一版)。

## `--theme`: add a theme to the documentation output

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --theme /path/to/your/custom-theme.css
```

`rustdoc`的默认输出包含两个主题:`light`(默认)和`dark`。
此标志允许您向输出添加自定义主题。给出CSS将其作为附加主题选项添加到文档中。
主题的名称由其文件名决定;一个名为`custom-theme.css`
将添加一个名为`custom-theme`的主题到文档中。

## `--check-theme`: verify custom themes against the default theme

使用该flag的方式如下：

```bash
$ rustdoc --check-theme /path/to/your/custom-theme.css
```

虽然 `rustdoc`的HTML输出在不同版本之间或多或少是一致的
不能保证主题文件具有相同的效果。`--theme`标志
仍然允许您将主题添加到文档中，但要确保这一点
您的主题按预期工作，您可以使用此标志来验证它是否实现
与官方的`light`主题相同的CSS规则。

`--check-theme` 在`rustdoc`中是一个单独的模式。当`rustdoc`看到
`--check-theme` 标志，它会丢弃所有其他标志，只执行CSS规则
比较操作。

### `--crate-version`: control the crate version

使用该flag的方式如下：

```bash
$ rustdoc src/lib.rs --crate-version 1.3.37
```

当`rustdoc`接收到这个标志时，它将在侧边栏中打印一个额外的"Version(版本)"
crate根目录的docs。您可以使用此标志来区分不同版本的
库的文档。

## `@path`: load command-line flags from a path

如果您在命令行中指定 `@path` ，那么它将打开 `path` 并读取
命令行选项。这些选项每行一个;空行表示
空选项。该文件可以使用Unix或Windows样式的行结尾，并且必须是
编码为UTF-8。

## `--passes`: add more rustdoc passes

This flag is **deprecated**.
For more details on passes, see [the chapter on them](passes.md).

## `--no-defaults`: don't run default passes

This flag is **deprecated**.
For more details on passes, see [the chapter on them](passes.md).

## `-r`/`--input-format`: input format

This flag is **deprecated** and **has no effect**.

Rustdoc only supports Rust source code and Markdown input formats. If the
file ends in `.md` or `.markdown`, `rustdoc` treats it as a Markdown file.
Otherwise, it assumes that the input file is Rust.

## `--nocapture`

When this flag is used with `--test`, the output (stdout and stderr) of your tests won't be
captured by rustdoc. Instead, the output will be directed to your terminal,
as if you had run the test executable manually. This is especially useful
for debugging your tests!
