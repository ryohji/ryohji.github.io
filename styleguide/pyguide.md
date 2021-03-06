# Google Python Style Guide

Note that this copy is from Google's [work](https://google.github.io/styleguide/pyguide.html)
which is originally licensed under the [CC-By 3.0 License]((https://creativecommons.org/licenses/by/3.0/)),
and this copy inherits it too.  
これは Google が CC-By 3.0 ライセンスで公開しているガイドの日本語訳です。
原文と同様 CC-By 3.0 で公開します。  
![Creative Commons License](https://i.creativecommons.org/l/by/3.0/88x31.png)

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->

<details markdown="1">
  <summary>Table of Contents</summary>

* [1 Background](#1-background)
* [2 Python Language Rules](#2-python-language-rules)
  * [2.1 Lint](#21-lint)
  * [2.2 Imports](#22-imports)
  * [2.3 Packages](#23-packages)
  * [2.4 Exceptions](#24-exceptions)
  * [2.5 Global variables](#25-global-variables)
  * [2.6 Nested/Local/Inner Classes and Functions](#26-nestedlocalinner-classes-and-functions)
  * [2.7 Comprehensions & Generator Expressions](#27-comprehensions--generator-expressions)
  * [2.8 Default Iterators and Operators](#28-default-iterators-and-operators)
  * [2.9 Generators](#29-generators)
  * [2.10 Lambda Functions](#210-lambda-functions)
  * [2.11 Conditional Expressions](#211-conditional-expressions)
  * [2.12 Default Argument Values](#212-default-argument-values)
  * [2.13 Properties](#213-properties)
  * [2.14 True/False Evaluation](#214-truefalse-evaluations)
  * [2.16 Lexical Scoping](#216-lexical-scoping)
  * [2.17 Function and Method Decorators](#217-function-and-method-decorators)
  * [2.18 Threading](#218-threading)
  * [2.19 Power Features](#219-power-features)
  * [2.20 Modern Python: from \_\_future\_\_ imports](#220-modern-python-from-__future__-imports)
  * [2.21 Type Annotated Code](#221-type-annotated-code)
* [3 Python Style Rules](#3-python-style-rules)
  * [3.1 Semicolons](#31-semicolons)
  * [3.2 Line length](#32-line-length)
  * [3.3 Parentheses](#33-parentheses)
  * [3.4 Indentation](#34-indentation)
  * [3.5 Blank Lines](#35-blank-lines)
  * [3.6 Whitespace](#36-whitespace)
  * [3.7 Shebang Line](#37-shebang-line)
  * [3.8 Comments and Docstrings](#38-comments-and-docstrings)
  * [3.10 Strings](#310-strings)
  * [3.11 Files, Sockets, and similar Stateful Resources](#311-files-sockets-and-similar-stateful-resources)
  * [3.12 TODO Comments](#312-todo-comments)
  * [3.13 Imports formatting](#313-imports-formatting)
  * [3.14 Statements](#314-statements)
  * [3.15 Accessors](#315-accessors)
  * [3.16 Naming](#316-naming)
  * [3.17 Main](#317-main)
  * [3.18 Function length](#318-function-length)
  * [3.19 Type Annotations](#319-type-annotations)
* [4 Parting Words](#4-parting-words)

</details>

<a id="background"></a>
## 1 Background

背景

Python is the main dynamic language used at Google. This style guide is a list
of *dos and don'ts* for Python programs.  
Python は Google の主要な動的言語です。
このスタイルガイドは Python プログラムで *すべきこと、してはいけないこと* を挙げたものです。

To help you format code correctly, we've created a
[settings file for Vim](https://google.github.io/styleguide/google_python_style.vim).
For Emacs, the default settings should be fine.  
コードをただしく整形するために [Vim 設定ファイル](https://google.github.io/styleguide/google_python_style.vim)を用意しました。
Emacs は初期設定で充分です。

Many teams use the [yapf](https://github.com/google/yapf/)
auto-formatter to avoid arguing over formatting.  
たいていのチームは yapf 自動整形ツールをつかって、整形で紛糾しないようにしています。


<a id="python-language-rules"></a>
## 2 Python Language Rules

Python 言語に関わる規則

<a id="lint"></a>
### 2.1 Lint

Run `pylint` over your code using this [pylintrc](https://google.github.io/styleguide/pylintrc).  
コード全体に [pylintrc](https://google.github.io/styleguide/pylintrc) の設定で `pylint` をかけます。

#### 2.1.1 Definition

定義

`pylint`
is a tool for finding bugs and style problems in Python source code. It finds
problems that are typically caught by a compiler for less dynamic languages like
C and C++. Because of the dynamic nature of Python, some
warnings may be incorrect; however, spurious warnings should be fairly
infrequent.  
`pylint` は Python ソースコードにひそむバグや書式の問題をみつけるツールです。
これは C や C++ のような動的性の低い言語でコンパイラーがつかまえる類の問題をみつけてくれます。
Python の動的な性質のためにときに警告が不正確になりますが、間違うことはほとんどありません。

#### 2.1.2 Pros

利点

Catches easy-to-miss errors like typos, using-vars-before-assignment, etc.  
ミスタイプや定義前の変数参照といった見落としがちな間違いをみつけられます。

#### 2.1.3 Cons

欠点

`pylint`
isn't perfect. To take advantage of it, sometimes we'll need to write around it,
suppress its warnings or fix it.  
`pylint` は完璧ではありません。
結果を得るためには書き加えたり抑止したり修正したりといった作業が必要です。

#### 2.1.4 Decision

取り決め

Make sure you run
`pylint`
on your code.  
かならず `pylint` をコードに適用してください。

Suppress warnings if they are inappropriate so that other issues are not hidden.
To suppress warnings, you can set a line-level comment:  
不適切な警告は抑止します（ほかの問題は隠さないでください）。行単位のコメントで抑止します：

```python
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
                          # 拙いことだけど... pylint: disable=redefined-builtin
```

`pylint`
warnings are each identified by symbolic name (`empty-docstring`)
Google-specific warnings start with `g-`.  
`pylint` 警告には固有のシンボル名があります（`empty-docstring` など）。
Google が定義した警告名は `g-` ではじまります。

If the reason for the suppression is not clear from the symbolic name, add an
explanation.  
抑止した理由がシンボル名から判然としないなら説明をくわえてください。

Suppressing in this way has the advantage that we can easily search for
suppressions and revisit them.  
このように抑止していくことで抑止した箇所を探しやすく、そして再チェックしやすくなります。

You can get a list of
`pylint`
warnings by doing:  
`pylint` 警告はつぎのようにして一覧できます：

```shell
pylint --list-msgs
```

To get more information on a particular message, use:  
より詳細に知りたい場合は次のようにします：

```shell
pylint --help-msg=C6409
```

Prefer `pylint: disable` to the deprecated older form `pylint: disable-msg`.  
`pylint: disable` で抑止してください。（古い `pylint: disable-msg` はつかいません）

Unused argument warnings can be suppressed by deleting the variables at the
beginning of the function. Always include a comment explaining why you are
deleting it. "Unused." is sufficient. For example:  
未使用の引数の警告には関数冒頭で削除して対応してください。
削除理由もあわせてコメントしてください。「未使用」で充分です。たとえば：

```python
def viking_cafe_order(spam: str, beans: str, eggs: Optional[str] = None) -> str:
    del beans, eggs  # Unused by vikings.
    return spam + spam + spam
```

Other common forms of suppressing this warning include using '`_`' as the
identifier for the unused argument or prefixing the argument name with
'`unused_`', or assigning them to '`_`'. These forms are allowed but no longer
encouraged. These break callers that pass arguments by name and do not enforce
that the arguments are actually unused.  
警告に対応するその他の方法もあります。変数名を ‘`_`’ にしたり引数名に ‘`unused_`’
を前置したり ‘`_`’ に代入するなどです。
これら方法をつかってもかまいませんがお勧めしません。
これらの方法は引数名を指定した呼びだしを壊しますし、引数がつかわれないことも保証できません。


<a id="imports"></a>
### 2.2 Imports

インポート

Use `import` statements for packages and modules only, not for individual
classes or functions. Imports from the [typing module](#typing-imports),
[typing_extensions module](https://github.com/python/typing/tree/master/typing_extensions),
and the
[six.moves module](https://six.readthedocs.io/#module-six.moves)
are exempt from this rule.  
`import` 文はパッケージとモジュールのためだけにつかい、個々のクラスや関数を取り込むためにはつかいません。
ただし [typing モジュール](#typing-imports)、
[typing_extensions モジュール](https://github.com/python/typing/tree/master/typing_extensions)、
そして [six.moves モジュール](https://six.readthedocs.io/#module-six.moves)は例外とします。

#### 2.2.1 Definition

定義

Reusability mechanism for sharing code from one module to another.  
あるモジュールのコードを別モジュールと共有する再利用の仕組みです。

#### 2.2.2 Pros

利点

The namespace management convention is simple. The source of each identifier is
indicated in a consistent way; `x.Obj` says that object `Obj` is defined in
module `x`.  
名前空間は単純な方法で管理します。識別子のソースを一貫した方法で提示します。
たとえば `x.Obj` で、オブジェクト `Obj` はモジュール `x` に定義されているとわかります。

#### 2.2.3 Cons

欠点

Module names can still collide. Some module names are inconveniently long.  
モジュール名が衝突することがあります。長くて不便な名前のモジュールもあります。

#### 2.2.4 Decision

取り決め

*   Use `import x` for importing packages and modules.  
    `import x` はパッケージあるいはモジュールのインポートにつかう。
*   Use `from x import y` where `x` is the package prefix and `y` is the module
    name with no prefix.  
    `from x import y` は前置されるパッケージ名 `x` に対しモジュール名 `y`
    を前置なしで参照するためにつかう。
*   Use `from x import y as z` if two modules named `y` are to be imported or if
    `y` is an inconveniently long name.  
    `from x import y as z` は同名のモジュール `y` をインポートするとき、あるいは
    長く不便な名前の `y` につかう。
*   Use `import y as z` only when `z` is a standard abbreviation (e.g., `np` for
    `numpy`).  
    `import y as z` をつかうのは `z` が標準的な略語のときだけ。（たとえば `numpy`
    に対する `np`）

For example the module `sound.effects.echo` may be imported as follows:  
たとえば `sound.effects.echo` モジュールは次のようにインポートします：

```python
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```

Do not use relative names in imports. Even if the module is in the same package,
use the full package name. This helps prevent unintentionally importing a
package twice.  
相対名でインポートしないでください。
おなじパッケージ内のモジュールでも完全名をつかいます。
こうすると気づかずおなじパッケージを二度インポートする愚を避けられます。


<a id="packages"></a>
### 2.3 Packages

パッケージ

Import each module using the full pathname location of the module.  
モジュールの完全パス名でインポートします。

#### 2.3.1 Pros

利点

Avoids conflicts in module names or incorrect imports due to the module search
path not being what the author expected. Makes it easier to find modules.  
モジュール名の衝突を避けたり（モジュール検索パスが作者の意図とちがうために起きる）
不正確なインポートを無くせます。また、こうすることでモジュールを探しやすくなります。

#### 2.3.2 Cons

欠点

Makes it harder to deploy code because you have to replicate the package
hierarchy. Not really a problem with modern deployment mechanisms.  
パッケージ階層を複製せねばならず、コードを配備しづらくなります。
とはいえ近年の配備機構があれば、これはさほどの問題ではありません。

#### 2.3.3 Decision

取り決め

All new code should import each module by its full package name.  
あたらしく書くコードでは、かならずモジュールを完全なパッケージ名でインポートします。

Imports should be as follows:  
インポートは次のようにします：

Yes:  
こうです：

```python
# Reference absl.flags in code with the complete name (verbose).
# absl.flags の参照はコード中で完全名になります（冗長ですが）。
import absl.flags
from doctor.who import jodie

FLAGS = absl.flags.FLAGS
```

```python
# Reference flags in code with just the module name (common).
# flags への参照はコード中でモジュール名だけで済みます（一般的）。
from absl import flags
from doctor.who import jodie

FLAGS = flags.FLAGS
```

No: _(assume this file lives in `doctor/who/` where `jodie.py` also exists)_  
こうではありません：_（`doctor/who/` の下、 `jodie.py` とおなじ場所のファイルだとして）_

```python
# Unclear what module the author wanted and what will be imported.  The actual
# import behavior depends on external factors controlling sys.path.
# Which possible jodie module did the author intend to import?
# 作者がどのモジュールを期待したか、そして実際になにがインポート
# されるかが不明瞭。実際のインポートがどう働くかは sys.path を制御する
# 外部要因で決まります。インポートされうる jodie モジュールは作者が意図した
# とおりのものでしょうか？
import jodie
```

The directory the main binary is located in should not be assumed to be in
`sys.path` despite that happening in some environments. This being the case,
code should assume that `import jodie` refers to a third party or top level
package named `jodie`, not a local `jodie.py`.  
メインバイナリーが配置されるディレクトリーを、（ある環境ではそうだとしても）
`sys.path` ディレクトリー内に入るものと仮定してはいけません。
これは、コードが `import jodie` でサードパーティーの、あるいは最上位パッケージ
`jodie` を（ローカルにある `jodie.py` でなく）参照するつもりのときに問題になります。


<a id="exceptions"></a>
### 2.4 Exceptions

例外

Exceptions are allowed but must be used carefully.  
例外はつかって構いませんが注意してください。


#### 2.4.1 Definition

定義

Exceptions are a means of breaking out of normal control flow to handle errors
or other exceptional conditions.  
例外は通常の制御フローの外に出るためにつかいます。
そしてエラーの処理や例外状況への対処をします。


#### 2.4.2 Pros

利点

The control flow of normal operation code is not cluttered by error-handling
code. It also allows the control flow to skip multiple frames when a certain
condition occurs, e.g., returning from N nested functions in one step instead of
having to plumb error codes through.  
通常処理の制御フローがエラー処理コードでごちゃごちゃしません。
また特定条件下で複数のフレームを飛ばすような制御もできます。
たとえば N 段の関数呼びだしからエラーコードを逐一検査せず１ステップで戻れます。


#### 2.4.3 Cons

欠点

May cause the control flow to be confusing. Easy to miss error cases when making
library calls.  
制御フローがわかりづらくなります。ライブラリー呼びだしでのエラー処理を忘れがちです。


#### 2.4.4 Decision

取り決め

Exceptions must follow certain conditions:  
例外は次の条件にしたがってください：

-   Make use of built-in exception classes when it makes sense. For example,
    raise a `ValueError` to indicate a programming mistake like a violated
    precondition (such as if you were passed a negative number but required a
    positive one). Do not use `assert` statements for validating argument values
    of a public API. `assert` is used to ensure internal correctness, not to
    enforce correct usage nor to indicate that some unexpected event occurred.
    If an exception is desired in the latter cases, use a raise statement. For
    example:  
    組み込みの例外は、その意味を活かしてつかう。たとえば `ValueError` で、
    コードを間違えて事前条件に違反したことを伝えられる
    （正の値が必要な処理で負の値を受けとったなど）。
    公開 API での引数確認のために `assert` をつかわない。 `assert`
    は内部的なただしさを確認するためにつかい、
    ただしい利用方法を強調したり不測の事態を知らせるためにはつかわない。
    例外で不測の事態をつたえるなら raise 文をつかう。たとえば：

    ```python
    Yes:
      def connect_to_next_port(self, minimum: int) -> int:
        """Connects to the next available port.

        Args:
          minimum: A port value greater or equal to 1024.

        Returns:
          The new minimum port.

        Raises:
          ConnectionError: If no available port is found.
        """
        if minimum < 1024:
          # Note that this raising of ValueError is not mentioned in the doc
          # string's "Raises:" section because it is not appropriate to
          # guarantee this specific behavioral reaction to API misuse.
          # ValueError を送出することは docstring の Raises セクションに
          # 書いていない。これは API を誤ってつかわれたときのふるまいを
          # 特に保証するのは不適当だからである。
          raise ValueError(f'Min. port must be at least 1024, not {minimum}.')
        port = self._find_next_open_port(minimum)
        if not port:
          raise ConnectionError(
              f'Could not connect to service on port {minimum} or higher.')
        assert port >= minimum, (
            f'Unexpected port {port} when minimum was {minimum}.')
        return port
    ```

    ```python
    No:
      def connect_to_next_port(self, minimum: int) -> int:
        """Connects to the next available port.

        Args:
          minimum: A port value greater or equal to 1024.

        Returns:
          The new minimum port.
        """
        assert minimum >= 1024, 'Minimum port must be at least 1024.'
        port = self._find_next_open_port(minimum)
        assert port is not None
        return port
    ```

-   Libraries or packages may define their own exceptions. When doing so they
    must inherit from an existing exception class. Exception names should end in
    `Error` and should not introduce stutter (`foo.FooError`).  
    ライブラリーやパッケージで独自の例外を定義できる。このときは既存の例外クラスを継承する。
    例外名は `Error` で終わらせる。また `foo.FooError` のような冗長な繰り返しを避ける。

-   Never use catch-all `except:` statements, or catch `Exception` or
    `StandardError`, unless you are  
    すべてを飲み込む `except:` 文や `Exception` や `StandardError`
    の捕捉は決してつかわない。ただし、

    -   re-raising the exception, or  
        その例外を再送出する場合や、
    -   creating an isolation point in the program where exceptions are not
        propagated but are recorded and suppressed instead, such as protecting a
        thread from crashing by guarding its outermost block.  
        例外を伝播せず記録して抑止するようなプログラムの隔離場所をつくるとき
        （たとえばスレッドの最外ブロックでクラッシュを防止する）は除く。  

    Python is very tolerant in this regard and `except:` will really catch
    everything including misspelled names, sys.exit() calls, Ctrl+C interrupts,
    unittest failures and all kinds of other exceptions that you simply don't
    want to catch.  
    Python はこのような例外捕捉に従順なうえ `except:` はあらゆるものを捕まえてしまう。
    たとえば名前の間違い、 sys.exit() 呼びだし、 Ctrl+C による割り込み、ユニットテストの失敗、
    その他よもや捕まえようとはおもわないあらゆる例外が対象となる。

-   Minimize the amount of code in a `try`/`except` block. The larger the body
    of the `try`, the more likely that an exception will be raised by a line of
    code that you didn't expect to raise an exception. In those cases, the
    `try`/`except` block hides a real error.  
    `try`/`except` ブロックで囲むコードをできるだけ短くする。
    `try` の本文が大きくなるほど、望まなかったコード行までが例外を投げるようになる。
    こうなると `try`/`except` ブロックが本当のエラーを隠してしまう。

-   Use the `finally` clause to execute code whether or not an exception is
    raised in the `try` block. This is often useful for cleanup, i.e., closing a
    file.  
    `finally` 節には `try` ブロック内での例外の発生如何によらず実行したいコードを書く。
    `finally` はファイルを閉じるといった後片付けに役立つ。


<a id="global-variables"></a>
### 2.5 Global variables

大域変数

Avoid global variables.  
大域変数はつかいません。


#### 2.5.1 Definition

定義

Variables that are declared at the module level or as class attributes.  
モジュールレベルあるいはクラスの属性として宣言された変数のことです。
（訳注：クラスの属性はメソッドの外で定義するクラスの変数です）

#### 2.5.2 Pros

利点

Occasionally useful.  
ときとして便利です。

#### 2.5.3 Cons

欠点

Has the potential to change module behavior during the import, because
assignments to global variables are done when the module is first imported.  
インポートの間にモジュールのふるまいを変えてしまうことがあります。
大域変数はモジュールの初回インポートで代入（設定）されるためです。

#### 2.5.4 Decision

取り決め

Avoid global variables.  
大域変数はつかいません。

While they are technically variables, module-level constants are permitted and
encouraged. For example: `_MAX_HOLY_HANDGRENADE_COUNT = 3`. Constants must be
named using all caps with underscores. See [Naming](#316-naming) below.  
技術的には変数ですが、モジュールレベルの定数は積極的につかいましょう。
たとえば `_MAX_HOLY_HANDGRANADE_COUNT = 3` といったものです。
定数名はすべて大文字とアンダースコア（キャピタルスネークケース）で構成します。
あとの[名づけ](#316-naming)も参照してください。

If needed, globals should be declared at the module level and made internal to
the module by prepending an `_` to the name. External access must be done
through public module-level functions. See [Naming](#316-naming) below.  
つかう場合は、大域変数はモジュールレベルで宣言し、またモジュール内に限定します。
名前に `_` を前置することでモジュール内部変数とできます。
外部からこれにアクセスするときはモジュールレベルの公開関数をつかいます。
[名づけ](#316-naming)も参照してください。


<a id="nested-classes-functions"></a>
### 2.6 Nested/Local/Inner Classes and Functions

入れ子の/ローカルの/内部のクラスと関数

Nested local functions or classes are fine when used to close over a local
variable. Inner classes are fine.  
ローカル変数を閉じこむために利用する入れ子のローカル関数やクラスはすばらしい。
内部クラスも最高です。（訳注： closure は関数呼びだし時点の「環境（変数とその値のペア）」
を閉じこむテクニック。時間のかかる計算を閉じこんでおき、あとで実行するなど）


#### 2.6.1 Definition

定義

A class can be defined inside of a method, function, or class. A function can be
defined inside a method or function. Nested functions have read-only access to
variables defined in enclosing scopes.  
クラスはメソッドや関数、クラスの中で定義できます。関数もメソッドや関数の中で定義できます。
入れ子の関数は、その関数の外のスコープの変数を読むこと（だけ）ができます。

#### 2.6.2 Pros

利点

Allows definition of utility classes and functions that are only used inside of
a very limited scope. Very
[ADT](https://en.wikipedia.org/wiki/Abstract_data_type)-y.
Commonly used for implementing decorators.  
限定されたスコープの中でだけつかえる補助クラスや関数を定義できます。非常に
[ADT](https://en.wikipedia.org/wiki/Abstract_data_type)
らしさがあります。（訳注：ADT は抽象データ型のこと。ひとそろいの操作としてデータを表現する）  
[デコレーター](#217-function-and-method-decorators)を定義するためによくつかわれます。

#### 2.6.3 Cons

欠点

Nested functions and classes cannot be directly tested. Nesting can make the
outer function longer and less readable.  
入れ子の関数やクラスは直接テストできません。入れ子にすることで外側の関数は長くなり、
そのぶん読みにくくなります。

#### 2.6.4 Decision

取り決め

They are fine with some caveats. Avoid nested functions or classes except when
closing over a local value other than `self` or `cls`. Do not nest a function
just to hide it from users of a module. Instead, prefix its name with an \_ at
the module level so that it can still be accessed by tests.  
いくつか注意は必要ですがすばらしいものです。
（`self` と `cls` を除く）ローカル変数を閉じこむ以外の目的で関数やクラスを入れ子にしてはいけません。
関数をモジュールの利用者から隠すために入れ子にしてはいけません。
この目的ならば \_ を前置してモジュールレベルで定義します（これならテストもできます）。


<a id="comprehensions"></a>
### 2.7 Comprehensions & Generator Expressions

内包表記とジェネレーター

Okay to use for simple cases.  
簡単なつかいかたなら構いません。

#### 2.7.1 Definition

定義

List, Dict, and Set comprehensions as well as generator expressions provide a
concise and efficient way to create container types and iterators without
resorting to the use of traditional loops, `map()`, `filter()`, or `lambda`.  
リストや辞書、集合の内包表記とジェネレーター式をつかうと、ループや `map()`、
`filter()`、 `lambda` をつかう従来の方法より、
わかりやすく効率的にコンテナ型やイテレーターを作成できます。

#### 2.7.2 Pros

利点

Simple comprehensions can be clearer and simpler than other dict, list, or set
creation techniques. Generator expressions can be very efficient, since they
avoid the creation of a list entirely.  
単純な内包表記は、 dict や list、 set のこれ以外の作成法より明瞭で簡明になります。
ジェネレーター式はリスト全体を生成せずにすむため効率的でもあります。


#### 2.7.3 Cons

欠点

Complicated comprehensions or generator expressions can be hard to read.  
複雑な内包表記やジェネレーター式は読みづらくなります。

#### 2.7.4 Decision

取り決め

Okay to use for simple cases. Each portion must fit on one line: mapping
expression, `for` clause, filter expression. Multiple `for` clauses or filter
expressions are not permitted. Use loops instead when things get more
complicated.  
単純ならつかってよいです。
変換部、 `for` 節、 filter 式のそれぞれが一行に収まるようにしてください。
複数の `for` 節や filter 式をつかってはいけません。複雑になるようならループにします。

```python
Yes:
  # 全体が一行に収まっている
  result = [mapping_expr for value in iterable if filter_expr]

  # 変換部と for 節が一行目、 filter 式が二行目
  result = [{'key': value} for value in iterable
            if a_long_filter_expression(value)]

  result = [complicated_transform(x)
            for x in iterable if predicate(x)]

  descriptive_name = [
      transform({'key': key, 'value': value}, color='black')
      for key, value in generate_iterable(some_input)
      if complicated_condition_is_met(key, value)
  ]

  # for が複数になるならループに開く
  result = []
  for x in range(10):
      for y in range(5):
          if x * y > 10:
              result.append((x, y))

  # 辞書を内包表記で生成
  return {x: complicated_transform(x)
          for x in long_generator_function(parameter)
          if x is not None}

  # ジェネレーター式の例
  squares_generator = (x**2 for x in range(10))

  # set 生成の内包表記
  unique_names = {user.name for user in users if user is not None}

  # eat 関数にジェネレーターを渡す
  eat(jelly_bean for jelly_bean in jelly_beans
      if jelly_bean.color == 'black')
```
```python
No:
  # 変換部が二行に渡っている
  result = [complicated_transform(
                x, some_argument=x+1)
            for x in iterable if predicate(x)]

  # for 節が二重になっている
  result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

  # for 節が三重になっている上に filter 式もふたつ
  return ((x, y, z)
          for x in range(5)
          for y in range(5)
          if x != y
          for z in range(5)
          if y != z)
```
```python
  # （訳注：これだったら OK？）
  return ((x, y, z)
          for x, y, z in itertools.product(range(5), range(5), range(5))
          if x != y and y != z)
```


<a id="default-iterators-operators"></a>
### 2.8 Default Iterators and Operators

デフォルトイテレーターと演算子

Use default iterators and operators for types that support them, like lists,
dictionaries, and files.  
リストや辞書、ファイルなど、型に用意されている既定のイテレーターや演算子をつかいましょう。


#### 2.8.1 Definition

定義

Container types, like dictionaries and lists, define default iterators and
membership test operators ("in" and "not in").  
コンテナ型（辞書やリストなど）は既定のイテレーターや包含確認演算（“in” や
“not in”）を定義しています。

#### 2.8.2 Pros

利点

The default iterators and operators are simple and efficient. They express the
operation directly, without extra method calls. A function that uses default
operators is generic. It can be used with any type that supports the operation.  
規定のイテレーターや演算子は単純で効率的です。
これらは操作そのものを表現しており、あらためてメソッド呼び出す必要はありません。
既定の演算子で書いた関数は汎用的です。その演算子をもつ型すべてにつかえるのです。

#### 2.8.3 Cons

欠点

You can't tell the type of objects by reading the method names (e.g. `has_key()`
means a dictionary). This is also an advantage.  
メソッド名からだけではオブジェクトの型がわかりません（`has_key()`
は辞書を暗示するだけです）。これは利点でもあります。

#### 2.8.4 Decision

取り決め

Use default iterators and operators for types that support them, like lists,
dictionaries, and files. The built-in types define iterator methods, too. Prefer
these methods to methods that return lists, except that you should not mutate a
container while iterating over it.  
リストや辞書、ファイルなど、型に用意されている既定のイテレーターや演算子をつかいましょう。
組み込み型はイテレーターメソッドも定義しています。リストを返すメソッドでなく、
これらイテレーターを返すメソッドをつかってください。
ただし巡回中にコンテナを変更しないことを示すためにリストメソッドをつかうのはよいでしょう。

```python
Yes:  for key in adict: ...
      if key not in adict: ...
      if obj in alist: ...
      for line in afile: ...
      for k, v in adict.items(): ...
      for k, v in six.iteritems(adict): ...
```
```python
No:   for key in adict.keys(): ...   # 既定の演算子で充分
      if not adict.has_key(key): ...  # if key not in adict: で確認可能
      for line in afile.readlines(): ...  # for line in afile: で OK
      for k, v in dict.iteritems(): ...  # Python 3 以降 items() をつかう
```


<a id="generators"></a>
### 2.9 Generators

ジェネレーター

Use generators as needed.  
必要に応じてつかってください。

#### 2.9.1 Definition

定義

A generator function returns an iterator that yields a value each time it
executes a yield statement. After it yields a value, the runtime state of the
generator function is suspended until the next value is needed.  
ジェネレーター関数は `yield` 文を実行するたびに値を生成するイテレーターを返します。
関数は値を生成すると、次の値が要求されるまでそこで一旦停止状態になります。

#### 2.9.2 Pros

利点

Simpler code, because the state of local variables and control flow are
preserved for each call. A generator uses less memory than a function that
creates an entire list of values at once.  
呼びだしのたびにローカル変数と実行位置（訳注：原文は controll flow）の状態が保存されるため、
コードが簡単になります。リスト全体を生成する関数とくらべて、ジェネレーターはメモリーを消費しません。

#### 2.9.3 Cons

欠点

None.  
なし。

#### 2.9.4 Decision

取り決め

Fine. Use "Yields:" rather than "Returns:" in the docstring for generator
functions.  
すばらしい。
ジェネレーター関数の docstring では “Returns:” でなく “Yields:” をつかいます。


<a id="lambdas"></a>
### 2.10 Lambda Functions

無名関数

Okay for one-liners. Prefer generator expressions over `map()` or `filter()`
with a `lambda`.  
一行に収まるならつかってよいです。
`map()` や `filter()` で無名関数をつかうよりジェネレーター式をつかってください。

#### 2.10.1 Definition

定義

Lambdas define anonymous functions in an expression, as opposed to a statement.  
ラムダは文でなく式として無名関数を定義します。

#### 2.10.2 Pros

利点

Convenient.  
便利です。

#### 2.10.3 Cons

欠点

Harder to read and debug than local functions. The lack of names means stack
traces are more difficult to understand. Expressiveness is limited because the
function may only contain an expression.  
ローカル関数定義にくらべ読みづらく、デバッグしづらくなります。（関数に）名前がつかないため、
スタックトレースを理解しづらくなります。
（無名関数内に）ただひとつの式しか書けないため表現できるコードに限界があります。

#### 2.10.4 Decision

取り決め

Okay to use them for one-liners. If the code inside the lambda function is
longer than 60-80 chars, it's probably better to define it as a regular
[nested function](#lexical-scoping).  
一行に収まるならつかってよいです。無名関数内が 60 ～ 80 文字をこえるならば、
おそらく[入れ子の関数](#lexical-scoping)をつかうとよいでしょう。

For common operations like multiplication, use the functions from the `operator`
module instead of lambda functions. For example, prefer `operator.mul` to
`lambda x, y: x * y`.  
乗算のような一般的な演算は `operator` モジュール定義の関数をつかってください。
つまり `lambda x, y: x * y` と書くかわりに `operator.mul` をつかいます。
（訳注：たとえば `product` 関数を `product = lambda vs: functools.reduce(operator.mul, vs)` のように定義する）


<a id="conditional-expressions"></a>
### 2.11 Conditional Expressions

条件式

Okay for simple cases.  
簡単なものならつかってよいです。

#### 2.11.1 Definition

定義

Conditional expressions (sometimes called a “ternary operator”) are mechanisms
that provide a shorter syntax for if statements. For example: `x = 1 if cond
else 2`.  
条件式は（「三項演算子」とも呼ばれます） if 文をより短く書ける記法です。
たとえば `x = 1 if cond else 2` のようなものです。

#### 2.11.2 Pros

利点

Shorter and more convenient than an if statement.  
if 文より短く便利です。

#### 2.11.3 Cons

欠点

May be harder to read than an if statement. The condition may be difficult to
locate if the expression is long.  
if 文より読みづらくなります。式が長くなると条件を特定しづらくなります。

#### 2.11.4 Decision

取り決め

Okay to use for simple cases. Each portion must fit on one line:
true-expression, if-expression, else-expression. Use a complete if statement
when things get more complicated.  
簡単ならつかってよいです。真の式、 if の式、 else の式、それぞれが一行に収まるようにします。
複雑になるなら if 文にします。（訳注：先の `x = 1 if cond else 2` で言えば `1` が真の式、
`if cond` が if の式、 `else 2` が else の式）

```python
Yes:
    one_line = 'yes' if predicate(value) else 'no'
    slightly_split = ('yes' if predicate(value)
                      else 'no, nein, nyet')
    the_longest_ternary_style_that_can_be_done = (
        'yes, true, affirmative, confirmed, correct'
        if predicate(value)
        else 'no, false, negative, nay')
```
```python
No:
    bad_line_breaking = ('yes' if predicate(value) else
                         'no')
    portion_too_long = ('yes'
                        if some_long_module.some_long_predicate_function(
                            really_long_variable_name)
                        else 'no, false, negative, nay')
```


<a id="default-arguments"></a>
### 2.12 Default Argument Values

デフォルト引数

Okay in most cases.  
おおむね、つかってよいです。

#### 2.12.1 Definition

定義

You can specify values for variables at the end of a function's parameter list,
e.g., `def foo(a, b=0):`. If `foo` is called with only one argument, `b` is set
to 0. If it is called with two arguments, `b` has the value of the second
argument.  
たとえば `def foo(a, b=0):` のように関数の引数には値を指定できます（訳注：末尾から順に、間をあけずに）。
`foo` を引数ひとつで呼びだすと `b` は 0 になります。二引数で呼びだせば `b` はふたつめの値になります。

#### 2.12.2 Pros

利点

Often you have a function that uses lots of default values, but on rare
occasions you want to override the defaults. Default argument values provide an
easy way to do this, without having to define lots of functions for the rare
exceptions. As Python does not support overloaded methods/functions, default
arguments are an easy way of "faking" the overloading behavior.  
たくさんの規定値をほとんど変えずに呼べる関数をつくるとします。
デフォルト引数はこれを実現する簡単な方法です。
すなわち、まれな例外ケースのためにたくさんの関数をつくらずに済みます。
（訳注：引数ひとつ版、一個飛ばし版などのオーバーロードをたくさん定義せずにすむ）  
Python では関数やメソッドをオーバーロードできませんが、
デフォルト引数でオーバーロード的なふるまいを実現できます。

#### 2.12.3 Cons

欠点

Default arguments are evaluated once at module load time. This may cause
problems if the argument is a mutable object such as a list or a dictionary. If
the function modifies the object (e.g., by appending an item to a list), the
default value is modified.  
デフォルト引数はモジュールのロード時に一度だけ評価されます。
このため引数がリストや辞書のような変更可能なオブジェクトになっていると問題が起きます。
関数がオブジェクトを変更すると（リストに要素を追加するなど）デフォルト値が変わってしまうのです。

#### 2.12.4 Decision

Okay to use with the following caveat:  
次の警告にしたがう前提でつかってよいです。

Do not use mutable objects as default values in the function or method
definition.  
変更可能なオブジェクトを関数やメソッドの初期値にしない。

```python
Yes: def foo(a, b=None):
         if b is None:
             b = []
Yes: def foo(a, b: Optional[Sequence] = None):
         if b is None:
             b = []
Yes: def foo(a, b: Sequence = ()):  # Empty tuple OK since tuples are immutable
         ...
```
```python
No:  def foo(a, b=[]):
         ...
No:  def foo(a, b=time.time()):  # The time the module was loaded???
         ...
No:  def foo(a, b=FLAGS.my_thing):  # sys.argv has not yet been parsed...
         ...
No:  def foo(a, b: Mapping = {}):  # Could still get passed to unchecked code
         ...
```


<a id="properties"></a>
### 2.13 Properties

プロパティ

Use properties for accessing or setting data where you would normally have used
simple, lightweight accessor or setter methods.  
簡単で軽量な getter/setter をつかってきた場所では、データへのアクセス・設定にプロパティをつかいます。

#### 2.13.1 Definition

定義

A way to wrap method calls for getting and setting an attribute as a standard
attribute access when the computation is lightweight.  
（計算時間が軽い）メソッド呼びだしで属性の値の読み出しと設定をするところで、
これを通常の属性へのアクセスのようにラップする方法です。

#### 2.13.2 Pros

利点

Readability is increased by eliminating explicit get and set method calls for
simple attribute access. Allows calculations to be lazy. Considered the Pythonic
way to maintain the interface of a class. In terms of performance, allowing
properties bypasses needing trivial accessor methods when a direct variable
access is reasonable. This also allows accessor methods to be added in the
future without breaking the interface.  
簡単な属性アクセスの getter/setter メソッド呼びだしをなくせるため可読性が高まります。
計算を遅らせられます。クラスのインターフェースを維持する Python らしさが考慮されています。
性能面では、変数への直接アクセスがよさそうなら、些末なアクセッサーメソッドをなくすように実装することもできます。
将来、インターフェースを壊さずアクセッサーメソッドを追加することもできます。

#### 2.13.3 Cons

欠点

Can hide side-effects much like operator overloading. Can be confusing for
subclasses.  
演算子のオーバーロードのように副作用が見えづらくなります。
サブクラスの実装で混乱するかもしれません。

#### 2.13.4 Decision

取り決め

Use properties in new code to access or set data where you would normally have
used lightweight accessor or setter methods. Properties should be created with
the `@property` [decorator](#217-function-and-method-decorators).  
あたらしく書くコードではデータの読みとり、設定をするときにプロパティをつかいましょう
（これまでに軽量の getter/setter を書いていたところ）。プロパティは
`@property` [デコレーター](#217-function-and-method-decorators)で作成します。

Inheritance with properties can be non-obvious if the property itself is not
overridden. Thus one must make sure that accessor methods are called indirectly
to ensure methods overridden in subclasses are called by the property (using the
[template method design pattern](https://en.wikipedia.org/wiki/Template_method_pattern)).  
プロパティをつかった継承はプロパティそのものをオーバーライドしていないとわかりづらいかもしれません。
プロパティが、オーバーライドしたアクセッサーメソッドを間接的に呼びだしていないことを確かめなければならないからです
（[デザインパターンのテンプレートメソッド](https://en.wikipedia.org/wiki/Template_method_pattern)をつかっているときなど）。

```python
Yes: import math

     class Square:
         """A square with two properties: a writable area and a read-only perimeter.

         To use:
         >>> sq = Square(3)
         >>> sq.area
         9
         >>> sq.perimeter
         12
         >>> sq.area = 16
         >>> sq.side
         4
         >>> sq.perimeter
         16
         """

         def __init__(self, side: float):
             self.side = side

         @property
         def area(self) -> float:
             """Area of the square."""
             return self._get_area()

         @area.setter
         def area(self, area: float):
             self._set_area(area)

         def _get_area(self) -> float:
             """Indirect accessor to calculate the 'area' property."""
             return self.side ** 2

         def _set_area(self, area: float):
             """Indirect setter to set the 'area' property."""
             self.side = math.sqrt(area)

         @property
         def perimeter(self) -> float:
             return self.side * 4
```


<a id="truefalse-evaluations"></a>
### 2.14 True/False Evaluations

真と偽の評価

Use the "implicit" false if at all possible.  
可能なところではすべて「暗黙の」 false をつかいます。

#### 2.14.1 Definition

定義

Python evaluates certain values as `False` when in a boolean context. A quick
"rule of thumb" is that all "empty" values are considered false, so `0`, `None`,
`[]`, `{}`, `''` all evaluate as false in a boolean context.  
Python ではいくつかの値がブール値の文脈で `False` になります。
大雑把にいうと「空」の値は偽になります。たとえば `0`、 `None`、 `[]`、 `{}` や `’’`
はブール値として評価すると偽になります。

#### 2.14.2 Pros

利点

Conditions using Python booleans are easier to read and less error-prone. In
most cases, they're also faster.  
Python のブール値による条件式は読みやすく、また間違いを起こしにくくなっています。
ほとんどの場合、高速でもあります。

#### 2.14.3 Cons

欠点

May look strange to C/C++ developers.  
C/C++ 開発者には不思議におもえるかもしれません。

#### 2.14.4 Decision

取り決め

Use the "implicit" false if possible, e.g., `if foo:` rather than `if foo !=
[]:`. There are a few caveats that you should keep in mind though:  
可能なところではすべて「暗黙の」 false をつかいます。たとえば `if foo != []:` でなく
`if foo:` と書きます。ただしいくつか注意点がありますので覚えておいてください：

-   Always use `if foo is None:` (or `is not None`) to check for a `None` value.
    E.g., when testing whether a variable or argument that defaults to `None`
    was set to some other value. The other value might be a value that's false
    in a boolean context!  
    `None` はかならず `if foo is None` （あるいは `is not None`）で確認する。
    変数や引数のデフォルト値 `None` が、別の値かを確かめるときなど。
    `None` とおなじに、ほかにもブール値として偽と評価される値がある！
-   Never compare a boolean variable to `False` using `==`. Use `if not x:`
    instead. If you need to distinguish `False` from `None` then chain the
    expressions, such as `if not x and x is not None:`.  
    ブール変数を `False` と `==` で比較せず、代わりに `if not x:` とする。
    `False` と `None` を判別するには `if not x and x is not None:`
    のように式をつなぐ。
-   For sequences (strings, lists, tuples), use the fact that empty sequences
    are false, so `if seq:` and `if not seq:` are preferable to `if len(seq):`
    and `if not len(seq):` respectively.  
    並び（文字列、リスト、タプル）では、空の並びが偽であることを利用して `if seq:`
    あるいは `if not seq:` のように書く。（`if len(seq):` や `if not len(seq):`
    より望ましい）
-   When handling integers, implicit false may involve more risk than benefit
    (i.e., accidentally handling `None` as 0). You may compare a value which is
    known to be an integer (and is not the result of `len()`) against the
    integer 0.  
    整数の処理では暗黙の偽への変換は利点より危険が大きい
    （つまり `None` を誤って 0 と処理する、など）。整数になることが確実な値は 0
    と比較したほうがよい。（ただし `len()` の結果は除く）

    ```python
    Yes: if not users:
             print('no users')

         if foo == 0:
             self.handle_zero()

         if i % 10 == 0:
             self.handle_multiple_of_ten()

         def f(x=None):
             if x is None:
                 x = []
    ```

    ```python
    No:  if len(users) == 0:
             print('no users')

         if foo is not None and not foo:
             self.handle_zero()

         if not i % 10:
             self.handle_multiple_of_ten()

         def f(x=None):
             x = x or []
    ```

-   Note that `'0'` (i.e., `0` as string) evaluates to true.  
    （数値の `0` でなく文字列の） `'0'` は真になることに注意。


### 2.16 Lexical Scoping

文脈スコープ

Okay to use.  
つかってヨシ！


<a id="lexical-scoping"></a>
#### 2.16.1 Definition

定義

A nested Python function can refer to variables defined in enclosing functions,
but cannot assign to them. Variable bindings are resolved using lexical scoping,
that is, based on the static program text. Any assignment to a name in a block
will cause Python to treat all references to that name as a local variable, even
if the use precedes the assignment. If a global declaration occurs, the name is
treated as a global variable.  
入れ子の Python 関数はそれを囲む関数の変数を参照できます（ただし代入はできません）。
変数の束縛は文脈スコープ、つまりプログラムの字面どおりに解決されます。
ブロック内でのある名前への代入は Python ではローカル変数への参照あつかいになります
（代入に先行して利用していたとしても）。 global 宣言があるとその名前は大域変数になります。
（訳注： `global summand1` と宣言すると `summand1` は代入可能な大域変数になる）

An example of the use of this feature is:  
文脈スコープとは次のようなものです：

```python
def get_adder(summand1: float) -> Callable[[float], float]:
    """Returns a function that adds numbers to a given number."""
    def adder(summand2: float) -> float:
        return summand1 + summand2

    return adder
```

#### 2.16.2 Pros

利点

Often results in clearer, more elegant code. Especially comforting to
experienced Lisp and Scheme (and Haskell and ML and ...) programmers.  
簡潔で洗練されたコードになります。とくに熟練した関数型言語つかい（Lisp や Scheme、 Haskell、 ML など）
には心地よいでしょう。

#### 2.16.3 Cons

欠点

Can lead to confusing bugs. Such as this example based on
[PEP-0227](http://www.google.com/url?sa=D&q=http://www.python.org/dev/peps/pep-0227/):  
見分けづらいバグを生みます。
以下は [PEP-0227](http://www.google.com/url?sa=D&q=http://www.python.org/dev/peps/pep-0227/) をもとにした例です：

```python
i = 4
def foo(x: Iterable[int]):
    def bar():
        print(i, end='')
    # ...
    # A bunch of code here
    # ...
    for i in x:  # Ah, i *is* local to foo, so this is what bar sees
                 # ここで i が　foo のローカル変数になった。 bar はこれを参照する
        print(i, end='')
    bar()
```

So `foo([1, 2, 3])` will print `1 2 3 3`,
not `1 2 3 4`.  
よって `foo([1, 2, 3])` の呼びだしは `1 2 3 3` を表示します。（`1 2 3 4` にはなりません）

#### 2.16.4 Decision

取り決め

Okay to use.  
おつかいください。

<a id="function-and-method-decorators"></a>
<a id="decorators"></a>
### 2.17 Function and Method Decorators

関数とメソッドのデコレーター

Use decorators judiciously when there is a clear advantage. Avoid `staticmethod`
and limit use of `classmethod`.  
明確な利点があると判断できるときにデコレーターをつかいます。
`staticmethod` はつかわないでください。 `classmethod` の利用は控えてください。

#### 2.17.1 Definition

定義

[Decorators for Functions and Methods](https://docs.python.org/3/glossary.html#term-decorator)
(a.k.a "the `@` notation"). One common decorator is `@property`, used for
converting ordinary methods into dynamically computed attributes. However, the
decorator syntax allows for user-defined decorators as well. Specifically, for
some function `my_decorator`, this:  
[関数とメソッドのデコレーター](https://docs.python.org/3/glossary.html#term-decorator)のとおりです
（「`@` 記法」ともいう）。 `@property` が有名です。これはふつうのメソッドを動的に計算する属性に変換します。
ところでデコレーターの文法はユーザー定義のデコレーターをつくるためにもつかえます。
たとえばある関数 `my_decorator` があるとして、つぎのようにつかうと：

```python
class C:
    @my_decorator
    def method(self):
        # method body ...
```

is equivalent to:  
これはつぎとおなじ効果をもちます：

```python
class C:
    def method(self):
        # method body ...
    method = my_decorator(method)
```

#### 2.17.2 Pros

利点

Elegantly specifies some transformation on a method; the transformation might
eliminate some repetitive code, enforce invariants, etc.  
メソッドへのある種の変換をエレガントに表現できます。この変換はコードの繰り返しをなくしたり、
不変条件の強化などのためにつかえます。

#### 2.17.3 Cons

欠点

Decorators can perform arbitrary operations on a function's arguments or return
values, resulting in surprising implicit behavior. Additionally, decorators
execute at import time. Failures in decorator code are pretty much impossible to
recover from.  
デコレーターは関数の引数や戻り値にたいしてあらゆる操作ができるため、
驚くような予期せぬ結果が生じます。加えてデコレーターはインポート時に処理されます。
デコレーターの実装に問題があると、ここから復帰することはほぼ不可能です。

#### 2.17.4 Decision

取り決め

Use decorators judiciously when there is a clear advantage. Decorators should
follow the same import and naming guidelines as functions. Decorator pydoc
should clearly state that the function is a decorator. Write unit tests for
decorators.  
明確な利点があると判断できるときだけデコレーターをつかいます。
デコレーターは関数と同じインポートと名前づけのガイドラインにしたがってください。
デコレーターの docstring では、それがデコレーターであると明記してください。
デコレーター（の定義）にはユニットテストを書きます。

Avoid external dependencies in the decorator itself (e.g. don't rely on files,
sockets, database connections, etc.), since they might not be available when the
decorator runs (at import time, perhaps from `pydoc` or other tools). A
decorator that is called with valid parameters should (as much as possible) be
guaranteed to succeed in all cases.  
デコレーターに外部への依存を持ちこまないでください（つまり、ファイルやソケット、
データベース接続などに頼らない）。これらはデコレーターの解釈時に利用できないかもしれません
（解釈はインポート時におこなわれ、 pydoc などのツールもインポートはします）。
ただしく呼びだされたデコレーターはいかなるときも必ず成功するようにしてください。

Decorators are a special case of "top level code" - see [main](#317-main) for
more discussion.  
デコレーターは「トップレベルコード」の特殊な場合です。詳細は [main](#317-main)
の節を参照してください。

Never use `staticmethod` unless forced to in order to integrate with an API
defined in an existing library. Write a module level function instead.  
`staticmethod` の利用は禁止です。
（既存のライブラリーに定義された API と結合するために指定されている場合は除きます）  
代わりにモジュールレベルの関数を定義してください。

Use `classmethod` only when writing a named constructor or a class-specific
routine that modifies necessary global state such as a process-wide cache.  
`classmethod` は名前つきコンストラクターや、
プロセスワイドなキャッシュのような大域的状態の変更が避けられないクラス固有の処理を定義するときに（のみ）つかいます。


<a id="threading"></a>
### 2.18 Threading

マルチスレッド

Do not rely on the atomicity of built-in types.  
組み込み型の不可分性にたよらないでください。

While Python's built-in data types such as dictionaries appear to have atomic
operations, there are corner cases where they aren't atomic (e.g. if `__hash__`
or `__eq__` are implemented as Python methods) and their atomicity should not be
relied upon. Neither should you rely on atomic variable assignment (since this
in turn depends on dictionaries).  
辞書のような Python 組み込みデータ型が不可分操作を提供しているように見えても、
これらには不可分ではないエッジケースがあります（`__hash__` や `__eq__` が Python
のメソッドとして実装されている場合など）。そしてその不可分性をあてにしてはならないのです。
もちろん代入の不可分性もあてにしてはいけません（それが辞書になっていたとしてもです）。

Use the Queue module's `Queue` data type as the preferred way to communicate
data between threads. Otherwise, use the threading module and its locking
primitives. Prefer condition variables and `threading.Condition` instead of
using lower-level locks.  
スレッド間でのデータの受けわたしには Queue モジュールの `Queue` データ型をつかいましょう。
そうでなければ threading モジュールと、これのロック用プリミティブをつかいます。
その他の低レベルロックでなく条件変数 `threading.Condition` をつかってください。


<a id="power-features"></a>
### 2.19 Power Features

上級者機能

Avoid these features.  
つかわないでください。

#### 2.19.1 Definition

定義

Python is an extremely flexible language and gives you many fancy features such
as custom metaclasses, access to bytecode, on-the-fly compilation, dynamic
inheritance, object reparenting, import hacks, reflection (e.g. some uses of
`getattr()`), modification of system internals, `__del__` methods implementing
customized cleanup, etc.  
Python は柔軟性に富んでいます。カスタム・メタクラス、バイトコードへのアクセス、
実行時コンパイル、動的継承、親の更新、インポートハック、リフレクション（`getattr()`
の応用）、システム内部の書きかえ、 `__del__` メソッドによるクリーンアップの改造など、
めくるめく機能が目白押しです。

#### 2.19.2 Pros

利点

These are powerful language features. They can make your code more compact.  
強力な言語機能です。コードをより小さくできます。

#### 2.19.3 Cons

欠点

It's very tempting to use these "cool" features when they're not absolutely
necessary. It's harder to read, understand, and debug code that's using unusual
features underneath. It doesn't seem that way at first (to the original author),
but when revisiting the code, it tends to be more difficult than code that is
longer but is straightforward.  
とりたてて必要がなくてもつかいたくなるくらい「魅力的」な機能です。
しかし背後で一般的でない機能をつかったコードは、読みづらく、把握しづらく、
デバッグしづらくなります。最初はそうは見えません（コードの原著者には）が、
ふたたび読みかえしてみると、長めであっても直截に書かれたコードより難しく感じられるものです。

#### 2.19.4 Decision

取り決め

Avoid these features in your code.  
つかわないでください。

Standard library modules and classes that internally use these features are okay
to use (for example, `abc.ABCMeta`, `dataclasses`, and `enum`).  
標準ライブラリー定義のモジュールや、これらを内部的につかうクラスはつかって構いません。
（たとえば `abc.ABCMeta` や `dataclasses`、 `enum` など）


<a id="modern-python"></a>
### 2.20 Modern Python: from \_\_future\_\_ imports

Python 新機能: `__future__` インポート

New language version semantic changes may be gated behind a special future
import to enable them on a per-file basis within earlier runtimes.  
新バージョンで加わる言語的変更は、それより前バージョンのランタイムでも future
特殊インポートによりファイル単位で有効にできます。

#### 2.20.1 Definition

定義

Being able to turn on some of the more modern features via `from __future__
import` statements allows early use of features from expected future Python
versions.  
`from __future__ import` 文で新機能を有効にすると、 Python
の将来バージョンでの機能を先行してつかえます。

#### 2.20.2 Pros

利点

This has proven to make runtime version upgrades smoother as changes can be made
on a per-file basis while declaring compatibility and preventing regressions
within those files. Modern code is more maintainable as it is less likely to
accumulate technical debt that will be problematic during future runtime
upgrades.  
ランタイムを円滑にアップグレードできます。ファイル単位で変更できますし、
これらファイル中で互換性を定義したりバグを回帰させないよう対策できるためです。
コードを近代化することで保守性も高まります。
将来のランタイム更新に禍根を残すだろう技術的負債の蓄積を解消することにもなるからです。

#### 2.20.3 Cons

欠点

Such code may not work on very old interpreter versions prior to the
introduction of the needed future statement. The need for this is more common in
projects supporting an extremely wide variety of environments.  
future 文がつかえるようになる前のはるかに古いインタープリターではつかえません。
広範な環境をサポートするプロジェクトでは切実な問題です。

#### 2.20.4 Decision

取り決め

##### from __future__ imports

Use of `from __future__ import` statements is encouraged. It allows a given
source file to start using more modern Python syntax features today. Once you no
longer need to run on a version where the features are hidden behind a
`__future__` import, feel free to remove those lines.  
`from __future__ import` 文を活用してください。そのソースで、その日から近代的な
Python 文法をつかえます。 `__future__` インポートで機能を上書きするバージョンの対応が不要になったなら、
ためらわずにこの行の削除してください。

In code that may execute on versions as old as 3.5 rather than >= 3.7, import:  
バージョン 3.7 に満たない 3.5 程度に古い環境で実行するなら次をインポートしてください：

```python
from __future__ import generator_stop
```

For legacy code with the burden of continuing to support 2.7, import:  
2.7 のサポートを続ける重荷を負った古いコードでは次をインポートします：

```python
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function
```

For more information read the
[Python future statement definitions](https://docs.python.org/3/library/__future__.html)
documentation.  
さらに詳しくは [Python future 文](https://docs.python.org/3/library/__future__.html)の文書を参照してください。

Please don't remove these imports until you are confident the code is only ever
used in a sufficiently modern environment. Even if you do not currently use the
feature a specific future import enables in your code today, keeping it in place
in the file prevents later modifications of the code from inadvertently
depending on the older behavior.  
上述のインポートは、コードが十二分にあたらしい環境でしか実行されないと確信できるまでは削除しないでください。
future インポートで有効にした機能をつかっていないとしてもインポートは残してください。
のちの変更で予期しなかった古い挙動に依存しないようにするためです。

Use other `from __future__` import statements as you see fit. We did not include
`unicode_literals` in our recommendations for 2.7 as it was not a clear win due
to implicit default codec conversion consequences it introduced in many places
within 2.7. Most dual-version 2-and-3 code was better off with explicit use of
`b''` and `u''` bytes and unicode string literals where necessary.  
このほかの `from __future__ import` は用途に応じてつかってください。
なお 2.7 の推奨インポートに `unicode_literals` を含めなかったのは、結果、
この暗黙の符号変換が 2.7 環境のいたるところで取りこまれるだろうと考えられ、
しかしこれを正当とする理由が見つけられなかったためです。
大半のバージョン 2 と 3 両対応のコードは、必要に応じて `b''` と `u''`
のバイト列とユニコード文字列リテラルを明示的につかいわけるとよいでしょう。

##### The six, future, and past libraries

When your project still needs to support use under both Python 2 and 3, use the
[six](https://pypi.org/project/six/),
[future](https://pypi.org/project/future/), and
[past](https://pypi.org/project/past/) libraries as you see fit. They exist to
make your code cleaner and life easier.  
プロジェクトが Python 2 と 3 の両対応をするなら、 [six](https://pypi.org/project/six/)、
[future](https://pypi.org/project/future/)、 [past](https://pypi.org/project/past/)
のいずれか、適切なライブラリーをつかってください。
これらはコードをきれいに、そして人生を楽にしてくれます。


<a id="typed-code"></a>
### 2.21 Type Annotated Code

型注釈つきコード

You can annotate Python 3 code with type hints according to
[PEP-484](https://www.python.org/dev/peps/pep-0484/), and type-check the code at
build time with a type checking tool like [pytype](https://github.com/google/pytype).  
Python 3 では [PEP-484](https://www.python.org/dev/peps/pep-0484/)
にしたがった型ヒントをつけられ、 [pytype](https://github.com/google/pytype)
などのツールでビルド時に型を確認できます。

Type annotations can be in the source or in a
[stub pyi file](https://www.python.org/dev/peps/pep-0484/#stub-files). Whenever
possible, annotations should be in the source. Use pyi files for third-party or
extension modules.  
型注釈はソースやスタブの pyi ファイルにつけられます。
できるかぎりソースで注釈をしてください。
Pyi ファイルをつかうのはサードパーティー製のモジュールや拡張モジュールの場合です。

#### 2.21.1 Definition

定義

Type annotations (or "type hints") are for function or method arguments and
return values:  
型注釈（あるいは「型ヒント」）は引数や戻り値のある関数・メソッドのためのものです：

```python
def func(a: int) -> List[int]:
```

You can also declare the type of a variable using similar
[PEP-526](https://www.python.org/dev/peps/pep-0526/) syntax:  
[PEP-526](https://www.python.org/dev/peps/pep-0526/) に類する文法で変数の型も指定できます。

```python
a: SomeType = some_func()
```

Or by using a type comment in code that must support legacy Python versions.  
ふるい Python をサポートするために型コメントにとどめることもあります。

```python
a = some_func()  # type: SomeType
```

#### 2.21.2 Pros

利点

Type annotations improve the readability and maintainability of your code. The
type checker will convert many runtime errors to build-time errors, and reduce
your ability to use [Power Features](#power-features).  
型注釈はコードの可読性と保守性を高めます。
型チェックをすると多くの実行時エラーをビルド時に検出できるようになり、
[上級者向け機能](#power-features)にたよる必要も薄れます。

#### 2.21.3 Cons

欠点

You will have to keep the type declarations up to date.
You might see type errors that you think are
valid code. Use of a
[type checker](https://github.com/google/pytype)
may reduce your ability to use [Power Features](#power-features).  
型の宣言を更新し続ける必要があります。
ただしいとおもうコードに型エラーがつくこともあるでしょう。
型チェッカーをつかうと[上級者向け機能](#power-features)をつかう能力が衰えるでしょう。

#### 2.21.4 Decision

取り決め

You are strongly encouraged to enable Python type analysis when updating code.
When adding or modifying public APIs, include type annotations and enable
checking via pytype in the build system. As static analysis is relatively new to
Python, we acknowledge that undesired side-effects (such as
wrongly
inferred types) may prevent adoption by some projects. In those situations,
authors are encouraged to add a comment with a TODO or link to a bug describing
the issue(s) currently preventing type annotation adoption in the BUILD file or
in the code itself as appropriate.  
コードを更新するときに Python の型解析を有効にすることを強くおすすめします。
あらたな公開 API の追加や更新の際は型注釈をつけ、ビルドシステムに pytype
を組みこんでチェックしましょう。静的解析は Python に取りいれられてまだ日が浅いため
（型推論を誤るなど）望ましくない副作用があり、いくつかのプロジェクトには適さないでしょう。
こういった場合でも TODO コメントを付したり、
ビルドファイルやコードそのものに型注釈適用をさまたげているバグチケットのリンクを書いたり、
適切に対応してください。


<a id="python-style-rules"></a>
## 3 Python Style Rules

Python 整形にかかわる規則


<a id="semicolons"></a>
### 3.1 Semicolons

セミコロン

Do not terminate your lines with semicolons, and do not use semicolons to put
two statements on the same line.  
行末にセミコロンをおいてはいけません。
ふたつの文を一行にまとめるためにセミコロンをつかってもいけません。


<a id="line-length"></a>
### 3.2 Line length

行の長さ

Maximum line length is *80 characters*.  
１行は最大で *80文字まで* とします。

Explicit exceptions to the 80 character limit:  
80文字を超えてよいのは以下にかぎります：

-   Long import statements.  
    長いインポート文。
-   URLs, pathnames, or long flags in comments.  
    コメント中の URL、ファイルパス名、長いフラグなど。
-   Long string module level constants not containing whitespace that would be
    inconvenient to split across lines such as URLs or pathnames.  
inconvenient to split across lines such as URLs or pathnames.  
    inconvenient to split across lines such as URLs or pathnames.  
    モジュールレベルの文字列定数で空白をふくんでおらず、 URL
    やファイルパス名のように行をまたいだ分割が適さないもの。
    -   Pylint disable comments. (e.g.: `# pylint: disable=invalid-name`)
        Pylint 停止コメントを付す（`# pylint: disable=invalid-name`）

Do not use backslash line continuation except for `with` statements requiring
three or more context managers.  
バックスラッシュによる行継続はつかいません。（例外として、３つ以上のコンテキスト・
マネージャーを必要とする `with` 文はバックスラッシュ継続を認めます）

Make use of Python's
[implicit line joining inside parentheses, brackets and braces](http://docs.python.org/reference/lexical_analysis.html#implicit-line-joining).
If necessary, you can add an extra pair of parentheses around an expression.  
[Python の括弧内の暗黙の行結合](http://docs.python.org/reference/lexical_analysis.html#implicit-line-joining)を活用します。
必要に応じて式をかこむ括弧を追加してください。

```python
Yes: foo_bar(self, width, height, color='black', design=None, x='foo',
             emphasis=None, highlight=0)

     if (width == 0 and height == 0 and
         color == 'red' and emphasis == 'strong'):
```

When a literal string won't fit on a single line, use parentheses for implicit
line joining.  
文字列リテラルが一行におさまらなければ括弧による行結合をつかいます。

```python
x = ('This will build a very long long '
     'long long long long long long string')
```

Within comments, put long URLs on their own line if necessary.  
コメント中では、必要に応じて長い URL のために１行つかってください。

```python
Yes:  # See details at
      # http://www.example.com/us/developer/documentation/api/content/v2.0/csv_file_name_extension_full_specification.html
```
```python
No:  # See details at
     # http://www.example.com/us/developer/documentation/api/content/\
     # v2.0/csv_file_name_extension_full_specification.html
```

It is permissible to use backslash continuation when defining a `with` statement
whose expressions span three or more lines. For two lines of expressions, use a
nested `with` statement:  
３行以上にまたがる `with` 文を定義するときはバックスラッシュでの行継続をつかってよいものとします。
２式までなら `with` 文を入れ子にしてください。

```python
Yes:  with very_long_first_expression_function() as spam, \
           very_long_second_expression_function() as beans, \
           third_thing() as eggs:
          place_order(eggs, beans, spam, beans)
```
```python
No:  with VeryLongFirstExpressionFunction() as spam, \
          VeryLongSecondExpressionFunction() as beans:
       PlaceOrder(beans, spam)
```
```python
Yes:  with very_long_first_expression_function() as spam:
          with very_long_second_expression_function() as beans:
              place_order(beans, spam)
```

Make note of the indentation of the elements in the line continuation examples
above; see the [indentation](#34-indentation) section for explanation.  
上記例中の継続行内要素のインデントについて注意してください。
詳細は[インデント](#34-indentation)の節を参照してください。

In all other cases where a line exceeds 80 characters, and the
[yapf](https://github.com/google/yapf/)
auto-formatter does not help bring the line below the limit, the line is allowed
to exceed this maximum. Authors are encouraged to manually break the line up per
the notes above when it is sensible.  
これ以外で、行が 80 文字を超えてしまい [yapf](https://github.com/google/yapf/)
での自動整形で制限内におさまらない場合は、これらはすべて制限を超過してよいものとします。
コードを書く人は上述の内容にしたがい理にかなった行わけをするよう努めてください。


<a id="parentheses"></a>
### 3.3 Parentheses

丸カッコ

Use parentheses sparingly.  
括弧は控えめに。

It is fine, though not required, to use parentheses around tuples. Do not use
them in return statements or conditional statements unless using parentheses for
implied line continuation or to indicate a tuple.  
タプルのまわりの括弧は、必要とはされていませんが、つけてください。
ただし return 文や条件文では不要です。（この場合も、
暗黙の行継続やタプルを明示するためなら利用可です）

```python
Yes: if foo:
         bar()
     while x:
         x = bar()
     if x and y:
         bar()
     if not x:
         bar()
     # For a 1 item tuple the ()s are more visually obvious than the comma.
     # 1 要素のタプルはコンマでなく () をつかうと見た目でもわかりやすくなります。
     onesie = (foo,)
     return foo
     return spam, beans
     return (spam, beans)
     for (x, y) in dict.items(): ...
```
```python
No:  if (x):
         bar()
     if not(x):
         bar()
     return (foo)
```


<a id="indentation"></a>
### 3.4 Indentation

インデント

Indent your code blocks with *4 spaces*.  
コードブロックは空白４つでインデントします。

Never use tabs or mix tabs and spaces. In cases of implied line continuation,
you should align wrapped elements either vertically, as per the examples in the
[line length](#32-line-length) section; or using a hanging indent of 4 spaces,
in which case there should be nothing after the open parenthesis or bracket on
the first line.  
タブ文字やタブと空白を混ぜてつかわないでください。意図的に行を継続している場合、
[行の長さ](#32-line-length)の節で例示したように各要素を縦にそろえてください。
もしくは４空白のぶらさげインデントをおきます。
この場合は最初のひらき括弧の後ろにはなにもおかないでください。

```python
Yes:   # Aligned with opening delimiter
       # ひらき括弧位置で揃える
       foo = long_function_name(var_one, var_two,
                                var_three, var_four)
       meal = (spam,
               beans)

       # Aligned with opening delimiter in a dictionary
       # 辞書でのひらき括弧ぞろえ
       foo = {
           'long_dictionary_key': value1 +
                                  value2,
           ...
       }

       # 4-space hanging indent; nothing on first line
       # ４空白ぶらさげインデント。最初の行にはなにも書かない
       foo = long_function_name(
           var_one, var_two, var_three,
           var_four)
       meal = (
           spam,
           beans)

       # 4-space hanging indent in a dictionary
       # 辞書での４空白ぶらさげインデント
       foo = {
           'long_dictionary_key':
               long_dictionary_value,
           ...
       }
```
```python
No:    # Stuff on first line forbidden
       # ぶらさげるなら最初の行に要素をいれてはダメ
       foo = long_function_name(var_one, var_two,
           var_three, var_four)
       meal = (spam,
           beans)

       # 2-space hanging indent forbidden
       # ぶらさげで２空白はダメ
       foo = long_function_name(
         var_one, var_two, var_three,
         var_four)

       # No hanging indent in a dictionary
       # ぶらさげインデントのない辞書
       foo = {
           'long_dictionary_key':
           long_dictionary_value,
           ...
       }
```

<a id="trailing_comma"></a>
<a id="trailing_commas"></a>
<a id="trailing-comma"></a>
#### 3.4.1 Trailing commas in sequences of items?

シーケンス末尾のコンマは？

Trailing commas in sequences of items are recommended only when the closing
container token `]`, `)`, or `}` does not appear on the same line as the final
element. The presence of a trailing comma is also used as a hint to our Python
code auto-formatter [YAPF](https://pypi.org/project/yapf/) to direct it to auto-format the container
of items to one item per line when the `,` after the final element is present.  
項目並びの末尾のコンマは、 `]`、 `)`、 `}` のコンテナ終端トークンが最終要素とおなじ行にないときにつかいます。
末尾コンマは Python コードの自動整形ツール [YAPF](https://pypi.org/project/yapf/)
へのヒントにもなっており、末尾コンマはコンテナの要素ごとに１行で整形する指示になります。

```python
Yes:   golomb3 = [0, 1, 3]
Yes:   golomb4 = [
           0,
           1,
           4,
           6,
       ]
```
```python
No:    golomb4 = [
           0,
           1,
           4,
           6
       ]
```


<a id="blank-lines"></a>
### 3.5 Blank Lines

空白行

Two blank lines between top-level definitions, be they function or class
definitions. One blank line between method definitions and between the `class`
line and the first method. No blank line following a `def` line. Use single
blank lines as you judge appropriate within functions or methods.  
（関数やクラス定義といった）トップレベル定義の間には２行の空行をはさみます。
メソッド定義の間とクラス宣言行と最初のメソッドの間には１行の空行をいれます。
`def` 行の後ろには空行をいれません。
関数やメソッドの途中には適切と感じられるなら１行の空行を入れてください。


<a id="whitespace"></a>
### 3.6 Whitespace

空白文字類

Follow standard typographic rules for the use of spaces around punctuation.  
句読点の前後にいれる空白は、標準的な書きかたルールにしたがってください。

No whitespace inside parentheses, brackets or braces.  
括弧の内側には空白をいれないようにします。

```python
Yes: spam(ham[1], {'eggs': 2}, [])
```
```python
No:  spam( ham[ 1 ], { 'eggs': 2 }, [ ] )
```

No whitespace before a comma, semicolon, or colon. Do use whitespace after a
comma, semicolon, or colon, except at the end of the line.  
コンマ、セミコロン、コロンの前には空白をおきません。コンマ、セミコロン、
コロンの後ろには空白をいれます。（ただし行末は除きます）

```python
Yes: if x == 4:
         print(x, y)
     x, y = y, x
```
```python
No:  if x == 4 :
         print(x , y)
     x , y = y , x
```

No whitespace before the open paren/bracket that starts an argument list,
indexing or slicing.  
引数リスト、インデックス、スライスを指定する開き括弧の前には空白をおきません。

```python
Yes: spam(1)
```

```python
No:  spam (1)
```

```python
Yes: dict['key'] = list[index]
```

```python
No:  dict ['key'] = list [index]
```

No trailing whitespace.  
行末には空白をおきません。

Surround binary operators with a single space on either side for assignment
(`=`), comparisons (`==`, `<`, `>`, `!=`, `<>`, `<=`, `>=`, `in`, `not in`,
`is`, `is not`), and Booleans (`and`, `or`, `not`). Use your better judgment
for the insertion of spaces around arithmetic operators (`+`, `-`, `*`, `/`,
`//`, `%`, `**`, `@`).  
二項演算子の両側に空白をひとつずつおきます。代入（`=`）、比較（`==`, `<`, `>`,
`!=`, `<>`, `<=`, `>=`, `in`, `not in`, `is`, `is not`）、ブール演算（`and`,
`or`, `not`）などです（訳注： `not` は単項演算子）。
数値演算子の周囲にいれる空白は読者のよりよい判断におまかせします（`+`, `-`, `*`,
`/`, `//`, `%`, `**`, `@`）。（訳注： `@` 演算子がなにかは不明）

```python
Yes: x == 1
```

```python
No:  x<1
```

Never use spaces around `=` when passing keyword arguments or defining a default
parameter value, with one exception:
[when a type annotation is present](#typing-default-values), _do_ use spaces
around the `=` for the default parameter value.  
キーワード引数とパラメーターの規定値を定義する `=` の両端には空白をおきません。
唯一の例外は[規定値つきの型注釈](#typing-default-values)をしているときで、
規定値定義の `=` の両端に空白をいれます。

```python
Yes: def complex(real, imag=0.0): return Magic(r=real, i=imag)
Yes: def complex(real, imag: float = 0.0): return Magic(r=real, i=imag)
```

```python
No:  def complex(real, imag = 0.0): return Magic(r = real, i = imag)
No:  def complex(real, imag: float=0.0): return Magic(r = real, i = imag)
```

Don't use spaces to vertically align tokens on consecutive lines, since it
becomes a maintenance burden (applies to `:`, `#`, `=`, etc.):  
複数行でトークンの桁をそろえるために空白をつかってはいけません。
のちの保守の重荷になるからです。（`:`, `#`, `=` などでの桁ぞろえも同様に不可です）

```python
Yes:
  foo = 1000  # comment
  long_name = 2  # comment that should not be aligned

  dictionary = {
      'foo': 1,
      'long_name': 2,
  }
```

```python
No:
  foo       = 1000  # comment
  long_name = 2     # comment that should not be aligned

  dictionary = {
      'foo'      : 1,
      'long_name': 2,
  }
```


<a id="shebang-line"></a>
### 3.7 Shebang Line

シェバン行

Most `.py` files do not need to start with a `#!` line. Start the main file of a
program with
`#!/usr/bin/env python3` (to support virtualenvs) or `#!/usr/bin/python3` per
[PEP-394](https://www.python.org/dev/peps/pep-0394/).  
ほとんどの `.py` ファイルは `#!` 行ではじめる必要はありません。
プログラムのメインファイルは（仮想環境に対応するなら） `#!/user/bin/env python3` で、
あるいは [PEP-394](https://www.python.org/dev/peps/pep-0394/) にしたがって
`#!/usr/bin/python3` ではじめます。

This line is used by the kernel to find the Python interpreter, but is ignored
by Python when importing modules. It is only necessary on a file intended to
be executed directly.  
この行はカーネルが Python インタープリターを見つけるためにつかうもので、
Python がモジュールをインポートするときにはこれを無視します。
これは直接実行する想定のファイルでのみ必要です。


<a id="documentation"></a>
### 3.8 Comments and Docstrings

コメントと docstring

Be sure to use the right style for module, function, method docstrings and
inline comments.  
モジュール、関数、メソッドの docstring やコード内コメントをただしく整形しましょう。

<a id="docstrings"></a>
#### 3.8.1 Docstrings

docstring

Python uses _docstrings_ to document code. A docstring is a string that is the
first statement in a package, module, class or function. These strings can be
extracted automatically through the `__doc__` member of the object and are used
by `pydoc`.
(Try running `pydoc` on your module to see how it looks.) Always use the three
double-quote `"""` format for docstrings (per
[PEP 257](https://www.google.com/url?sa=D&q=http://www.python.org/dev/peps/pep-0257/)).
A docstring should be organized as a summary line (one physical line not
exceeding 80 characters) terminated by a period, question mark, or exclamation
point. When writing more (encouraged), this must be followed by a blank line,
followed by the rest of the docstring starting at the same cursor position as
the first quote of the first line. There are more formatting guidelines for
docstrings below.  
Python はコードのドキュメントに _docstring_ をつかいます。 Docstring はパッケージ、
モジュール、クラスや関数の一番はじめの「文字列」です。この文字列は対象の `__doc__`
メンバーとして自動抽出され、これを `pydoc` がつかいます（どんな感じか自分のモジュールで
`pydoc` を試してみてください）。
Docstring は [PEP 257](https://www.google.com/url?sa=D&q=http://www.python.org/dev/peps/pep-0257/)
にしたがいダブルクォート三つ `"""` で囲みます。まずサマリー行からはじめます（行が
80 桁を超えないように）。サマリー行は句点、疑問符、感嘆符のいずれかで止めます。
さらに続けるときは（ぜひ書いてください）まず空行を入れます。
そしてサマリー行の開始引用符のカーソル位置にあわせて続きを書きます。
docstring 整形のガイドラインをつづけて紹介します。

<a id="module-docs"></a>
#### 3.8.2 Modules

モジュール

Every file should contain license boilerplate. Choose the appropriate
boilerplate for the license used by the project (for example, Apache 2.0,
BSD, LGPL, GPL)  
すべてのファイルにライセンスのボイラープレートを入れてください。
プロジェクトに適したボイラープレートを選びます。（Apache 2.0、 BSD、 LGPL、 GPL など）

Files should start with a docstring describing the contents and usage of the
module.  
ファイルは docstring ではじめます。これにはモジュールの内容と利用法を書きます。

```python
"""A one line summary of the module or program, terminated by a period.
モジュールやプログラムの説明を一行で。末尾の句点を忘れずに。

Leave one blank line.  The rest of this docstring should contain an
overall description of the module or program.  Optionally, it may also
contain a brief description of exported classes and functions and/or usage
examples.
一行あける。そしてモジュール（プログラム）全体を説明します。
公開するクラスや関数の簡単な説明や使用例を書いてもよいでしょう。

  Typical usage example:
  代表的なつかいかた：

  foo = ClassFoo()
  bar = foo.FunctionBar()
"""
```

<a id="function-docs"></a>
#### 3.8.3 Functions and Methods

関数とメソッド

In this section, "function" means a method, function, or generator.  
この節では「関数」はメソッド、関数、ジェネレーターすべてを指します。

A function must have a docstring, unless it meets all of the following criteria:  
関数にはすべて docstring を書きます。ただし以下の三つは例外とします:
-   not externally visible  
    外部には非公開
-   very short  
    ごく短い
-   obvious  
    単純明快

A docstring should give enough information to write a call to the function
without reading the function's code. The docstring should describe the
function's calling syntax and its semantics, but generally not its
implementation details, unless those details are relevant to how the function is
to be used. For example, a function that mutates one of its arguments as a side
effect should note that in its docstring. Otherwise, subtle but important
details of a function's implementation that are not relevant to the caller are
better expressed as comments alongside the code than within the function's
docstring.  
Docstring にはコードを読まずとも関数の呼びだしを書けるよう十分な情報を書きます。
呼びだしの文法、そしてその意味（なにが起きるか）は必ず書きます。
関数の利用法に関係しなければ実装の詳細を省略するのが通例です。
呼びだしの副作用として引数を変更するような関数は、これを注記すべきです。
関数の呼びだし元には関係しないけれど、重要でしかし気づきにくい実装の詳細は関数の
docstring でなくコードのコメントとして書くのがよいでしょう。

The docstring should be descriptive-style (`"""Fetches rows from a
Bigtable."""`) rather than imperative-style (`"""Fetch rows from a
Bigtable."""`). The docstring for a `@property` data descriptor should use the
same style as the docstring for an attribute or a
[function argument](#doc-function-args) (`"""The Bigtable path."""`,
rather than `"""Returns the Bigtable path."""`).  
Docstring は命令形（`"""Bigtable から複数行を取得せよ。"""`）でなく叙述的
（`"""Bigtable から複数行を取得する。"""`）に書きます。 `@property` の docstring
は属性や[関数引数](#doc-function-args)のように書きます
（`"""Bigtable パスを返す。"""` でなく `"""Bigtable パス。"""`）。

A method that overrides a method from a base class may have a simple docstring
sending the reader to its overridden method's docstring, such as `"""See base
class."""`. The rationale is that there is no need to repeat in many places
documentation that is already present in the base method's docstring. However,
if the overriding method's behavior is substantially different from the
overridden method, or details need to be provided (e.g., documenting additional
side effects), a docstring with at least those differences is required on the
overriding method.  
基底クラスの実装をオーバーライドしたメソッドは、オーバーライドした実装の docstring
を参照するよう（`"""基底クラスを参照。"""` のように）簡単に書いてかまいません。
基底クラスの docstring で説明ずみの内容をあちこちで繰りかえさなくてよかろうという理由です。
ただしオーバーライド元とは違った動きをするだとか、細かな捕捉が必要
（たとえば新たな副作用を説明する）だとかいうときには、 docstring にこういった違いを書いてください。

Certain aspects of a function should be documented in special sections, listed
below. Each section begins with a heading line, which ends with a colon. All
sections other than the heading should maintain a hanging indent of two or four
spaces (be consistent within a file). These sections can be omitted in cases
where the function's name and signature are informative enough that it can be
aptly described using a one-line docstring.  
以下に挙げたとおり、特にセクションをつくって説明すべき箇所があります。
各セクションは見出しではじめます（行末はコロン）。見出しの次からのセクションは２字、
あるいは４字分字下げしてください（字下げ幅はファイル内で統一）。関数名やその署名から一行の
docstring で理解するに足りるなら、これらセクションは省略してかまいません。

<a id="doc-function-args"></a>
[*Args:*](#doc-function-args)
:   List each parameter by name. A description should follow the name, and be
    separated by a colon followed by either a space or newline. If the
    description is too long to fit on a single 80-character line, use a hanging
    indent of 2 or 4 spaces more than the parameter name (be consistent with the
    rest of the docstrings in the file). The description should include required
    type(s) if the code does not contain a corresponding type annotation. If a
    function accepts `*foo` (variable length argument lists) and/or `**bar`
    (arbitrary keyword arguments), they should be listed as `*foo` and `**bar`.  
    名前でパラメーターを列挙します。名前に続けてコロンで区切りを入れ、一字あけるか改行して説明を書きます。
    説明が一行 80 文字に収まらないならパラメーター名より後ろになるよう２字あるいは４字ぶん字下げします
    （以降、ファイル内の docstring で統一）。対応する引数に型注釈を入れていないなら、
    説明に型情報を入れます。関数が `*foo` （可変長引数リスト）や `**bar` （任意のキーワード引数）
    を取るなら、これらも `*foo` や `**bar` の名前で列挙します。

<a id="doc-function-returns"></a>
[*Returns:* (or *Yields:* for generators)](#doc-function-returns)
:   Describe the type and semantics of the return value. If the function only
    returns None, this section is not required. It may also be omitted if the
    docstring starts with Returns or Yields (e.g. `"""Returns row from Bigtable
    as a tuple of strings."""`) and the opening sentence is sufficient to
    describe return value.  
    戻り値の型と意味を書きます。 None しか返さない関数ではこのセクションは不要です。
    返す（Returns）や生む（Yields）で docstring が終わる場合、
    そして戻り値を十分に説明しているときも省略してかまいません
    （`"""Bigtable から行を文字列の組として「返す」。"""`）。  
    （訳注： Returns や Yields ではじまる場合も、が原文。
    セクション名に相当するから省略可能ということだろう。日本語では省略不可としたほうがよさそう）

<a id="doc-function-raises"></a>
[*Raises:*](#doc-function-raises)
:   List all exceptions that are relevant to the interface followed by a
    description. Use a similar exception name + colon + space or newline and
    hanging indent style as described in *Args:*. You should not document
    exceptions that get raised if the API specified in the docstring is violated
    (because this would paradoxically make behavior under violation of the API
    part of the API).  
    インターフェースにかかわる例外を列挙し説明します。 *Args:* と同様、例外名 + コロン + 空白か改行、
    そして字下げの書式をつかいます。 Docstring の API 仕様に違反したことで発生する例外は書きません。
    （逆説的に API の誤ったつかいかたまでも API の仕様として定義することになります）

```python
def fetch_smalltable_rows(table_handle: smalltable.Table,
                          keys: Sequence[Union[bytes, str]],
                          require_all_keys: bool = False,
) -> Mapping[bytes, Tuple[str]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
        table_handle: An open smalltable.Table instance.
        keys: A sequence of strings representing the key of each table
          row to fetch.  String keys will be UTF-8 encoded.
        require_all_keys: Optional; If require_all_keys is True only
          rows with values set for all keys will be returned.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {b'Serak': ('Rigel VII', 'Preparer'),
         b'Zim': ('Irk', 'Invader'),
         b'Lrrr': ('Omicron Persei 8', 'Emperor')}

        Returned keys are always bytes.  If a key from the keys argument is
        missing from the dictionary, then that row was not found in the
        table (and require_all_keys must have been False).

    Raises:
        IOError: An error occurred accessing the smalltable.
    """
```

Similarly, this variation on `Args:` with a line break is also allowed:  
以下のように `Args:` を改行してもかまいません。

```python
def fetch_smalltable_rows(table_handle: smalltable.Table,
                          keys: Sequence[Union[bytes, str]],
                          require_all_keys: bool = False,
) -> Mapping[bytes, Tuple[str]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
      table_handle:
        An open smalltable.Table instance.
      keys:
        A sequence of strings representing the key of each table row to
        fetch.  String keys will be UTF-8 encoded.
      require_all_keys:
        Optional; If require_all_keys is True only rows with values set
        for all keys will be returned.

    Returns:
      A dict mapping keys to the corresponding table row data
      fetched. Each row is represented as a tuple of strings. For
      example:

      {b'Serak': ('Rigel VII', 'Preparer'),
       b'Zim': ('Irk', 'Invader'),
       b'Lrrr': ('Omicron Persei 8', 'Emperor')}

      Returned keys are always bytes.  If a key from the keys argument is
      missing from the dictionary, then that row was not found in the
      table (and require_all_keys must have been False).

    Raises:
      IOError: An error occurred accessing the smalltable.
    """
```

<a id="class-docs"></a>
#### 3.8.4 Classes

クラス

Classes should have a docstring below the class definition describing the class.
If your class has public attributes, they should be documented here in an
`Attributes` section and follow the same formatting as a
[function's `Args`](#doc-function-args) section.  
クラスにはクラス定義の直後にクラスを説明する docstring を与えます。公開する属性があるなら
`Attributes` セクションを設けここに書きます。書式は[関数引数](#doc-function-args)と同様です。

```python
class SampleClass:
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam: bool = False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

<a id="comments"></a>
#### 3.8.5 Block and Inline Comments

ブロックコメントとインラインコメント

The final place to have comments is in tricky parts of the code. If you're going
to have to explain it at the next [code review](https://en.wikipedia.org/wiki/Code_review),
you should comment it now. Complicated operations get a few lines of comments
before the operations commence. Non-obvious ones get comments at the end of the
line.  
コメントをどこに残すかは難しい問題です。
次回の[コードレビュー](https://ja.wikipedia.org/wiki/%E3%82%B3%E3%83%BC%E3%83%89%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC)で説明せねばならないなら、
いまここに書くのがよいでしょう。複雑な操作は、それをはじめる前に数行のコメントを付します。
不明瞭な操作なら行末にコメントをつけます。

```python
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.
# 重みつき辞書をつかって i が配列のどこにあるかを探す。配列サイズと
# 配列中の最大値で位置を推定し、二分探索して正確な位置を求める。

if i & (i-1) == 0:  # True if i is 0 or a power of 2.
                    # 0 か 2 のべき乗なら真。
```

To improve legibility, these comments should start at least 2 spaces away from
the code with the comment character `#`, followed by at least one space before
the text of the comment itself.  
可読性のため、このような（行末）コメントはコードから最低でも二文字離してコメント文字
`#` を置きます。そしてコメント文と一文字ぶん空けます。

On the other hand, never describe the code. Assume the person reading the code
knows Python (though not what you're trying to do) better than you do.  
一方で、決してコードを説明しないでください。コードを読んでいる人はあなたよりも Python
に詳しいとおもってください。（あなたが Python でしようとしていることはともかく）  
（訳注：つまり「やりたいこと」をコメントする。「やっていること」は説明しない）

```python
# BAD COMMENT: Now go through the b array and make sure whenever i occurs
# the next element is i+1
# 拙いコメント： ここから配列 b を舐めて i の次の要素が i+1 であると確かめる。
```

<!-- The next section is copied from the C++ style guide. -->

<a id="spelling"></a>
<a id="punctuation"></a>
<a id="grammar"></a>
<a id="punctuation-spelling-grammar"></a>
#### 3.8.6 Punctuation, Spelling, and Grammar

句読点、綴り、文法

Pay attention to punctuation, spelling, and grammar; it is easier to read
well-written comments than badly written ones.  
句読点、綴り、文法に注意してください。よく書けたコメントは不出来なものより読みやすい。

Comments should be as readable as narrative text, with proper capitalization and
punctuation. In many cases, complete sentences are more readable than sentence
fragments. Shorter comments, such as comments at the end of a line of code, can
sometimes be less formal, but you should be consistent with your style.  
コメントは適切に大文字・小文字、句読点をつかい、ナレーションのように読みやすくあるべきです。
ほとんどの場合、断片の羅列より完全な一文であるほうが読みやすくなります。
コード行末に付すような短いコメントは時としてくだけがちですが、とにかく一貫性を保ってください。

Although it can be frustrating to have a code reviewer point out that you are
using a comma when you should be using a semicolon, it is very important that
source code maintain a high level of clarity and readability. Proper
punctuation, spelling, and grammar help with that goal.  
セミコロンであるべきところがコンマになっているとレビュアーに指摘されるのは腹立たしいでしょう。
けれどソースコードを高いレベルで明晰に、そして可読性を高く保つことは何にもまして重要です。
適切な句読点、綴り、文法はこの目的に沿うものです。


<a id="strings"></a>
### 3.10 Strings

文字列

Use an
[f-string](https://docs.python.org/3/reference/lexical_analysis.html#f-strings),
the `%` operator, or the `format` method for formatting strings, even when the
parameters are all strings. Use your best judgment to decide between `+` and `%`
(or `format`) though. Do not use `%` or the `format` method for pure
concatenation.  
文字列補間（[f 文字列](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)）、
`%` 演算子、あるいは `format` メソッドで文字列を整形してください。（引数がすべて文字列だとしても）  
`+` と `%` （か `format`）のいずれをつかうかは理性を働かせてください。
単純な連結には `%` や `format` メソッドをつかわないでください。

```python
Yes: x = a + b
     x = '%s, %s!' % (imperative, expletive)
     x = '{}, {}'.format(first, second)
     x = 'name: %s; score: %d' % (name, n)
     x = 'name: {}; score: {}'.format(name, n)
     x = f'name: {name}; score: {n}'
```

```python
No: x = '%s%s' % (a, b)  # use + in this case
    x = '{}{}'.format(a, b)  # use + in this case
    x = first + ', ' + second
    x = 'name: ' + name + '; score: ' + str(n)
```

Avoid using the `+` and `+=` operators to accumulate a string within a loop. In
some conditions, accumulating a string with addition can lead to quadratic
rather than linear running time. Although common accumulations of this sort may
be optimized on CPython, that is an implementation detail. The conditions under
which an optimization applies are not easy to predict and may change. Instead,
add each substring to a list and `''.join` the list after the loop terminates,
or write each substring to an `io.StringIO` buffer. These techniques
consistently have amortized-linear run time complexity.  
ループ中では `+` や `+=` で連結しないようにします。文字列の繰り返し連結したところ、
線形時間でなく二乗で遅くなった例もありました。この類の連結は CPython
で最適化されたりもするでしょうけれど、実装詳細に過ぎます。
最適化が効く条件は簡単に予測できませんし条件は変わりえます。
代わりに各部分文字列をリストに追加してループ終了時に `''.join` したり、
あるいは `io.StringIO` バッファーに逐次書きこんだりできます。
これらの実行時間複雑性は定数償却時間です。

```python
Yes: items = ['<table>']
     for last_name, first_name in employee_list:
         items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
     items.append('</table>')
     employee_table = ''.join(items)
```

```python
No: employee_table = '<table>'
    for last_name, first_name in employee_list:
        employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
    employee_table += '</table>'
```

Be consistent with your choice of string quote character within a file. Pick `'`
or `"` and stick with it. It is okay to use the other quote character on a
string to avoid the need to `\\ ` escape within the string.  
文字列の引用符はファイル内で統一します。 `'` か `"` 一方を選びます。
文字列中での `\\` エスケープを避けるためにもう一方の引用符をつかうのは問題ありません。

```python
Yes:
  Python('Why are you hiding your eyes?')
  Gollum("I'm scared of lint errors.")
  Narrator('"Good!" thought a happy Python reviewer.')
```

```python
No:
  Python("Why are you hiding your eyes?")
  Gollum('The lint. It burns. It burns us.')
  Gollum("Always the great lint. Watching. Watching.")
```

Prefer `"""` for multi-line strings rather than `'''`. Projects may choose to
use `'''` for all non-docstring multi-line strings if and only if they also use
`'` for regular strings. Docstrings must use `"""` regardless.  
複数行文字列では `"""` をつかってください。
通常の文字列に `'` を使用するプロジェクトでは複数行文字列での `'''` 利用を認めますが、
docstring では例外なく `"""` をつかいます。

Multi-line strings do not flow with the indentation of the rest of the program.
If you need to avoid embedding extra space in the string, use either
concatenated single-line strings or a multi-line string with
[`textwrap.dedent()`](https://docs.python.org/3/library/textwrap.html#textwrap.dedent)
to remove the initial space on each line:  
複数行文字列がプログラムのインデントを乱さないようにしてください。
文字列内に余計な空白をいれたくないなら、一行の文字列を連結するか
[`textwrap.dedent()`](https://docs.python.org/3/library/textwrap.html#textwrap.dedent)
で各行の先頭空白を取りのぞきます。

```python
  No:
  long_string = """This is pretty ugly.
Don't do this.
"""
```

```python
  Yes:
  long_string = """This is fine if your use case can accept
      extraneous leading spaces."""
```

```python
  Yes:
  long_string = ("And this is fine if you cannot accept\n" +
                 "extraneous leading spaces.")
```

```python
  Yes:
  long_string = ("And this too is fine if you cannot accept\n"
                 "extraneous leading spaces.")
```

```python
  Yes:
  import textwrap

  long_string = textwrap.dedent("""\
      This is also fine, because textwrap.dedent()
      will collapse common leading spaces in each line.""")
```

<a id="logging"></a>
#### 3.10.1 Logging

ログ出力

For logging functions that expect a pattern-string (with %-placeholders) as
their first argument: Always call them with a string literal (not an f-string!)
as their first argument with pattern-parameters as subsequent arguments. Some
logging implementations collect the unexpanded pattern-string as a queryable
field. It also prevents spending time rendering a message that no logger is
configured to output.  
（% プレースホルダー用の）パターン文字列を第一引数で受けとるログ出力関数では、
かならず文字列リテラル（f 文字列ではなく！）を一つ目に、そしてパターン引数をつづけて与えます。
クエリーフィールドとしてパターン文字列を展開せずに収集するログ実装もあります。
これによりログを出力しないよう設定しているときにメッセージを整形する時間を浪費せずに済みます。

```python
  Yes:
  import tensorflow as tf
  logger = tf.get_logger()
  logger.info('TensorFlow Version is: %s', tf.__version__)
```

```python
  Yes:
  import os
  from absl import logging

  logging.info('Current $PAGER is: %s', os.getenv('PAGER', default=''))

  homedir = os.getenv('HOME')
  if homedir is None or not os.access(homedir, os.W_OK):
    logging.error('Cannot write to home directory, $HOME=%r', homedir)
```

```python
  No:
  import os
  from absl import logging

  logging.info('Current $PAGER is:')
  logging.info(os.getenv('PAGER', default=''))

  homedir = os.getenv('HOME')
  if homedir is None or not os.access(homedir, os.W_OK):
    logging.error(f'Cannot write to home directory, $HOME={homedir!r}')
```

<a id="error-messages"></a>
#### 3.10.2 Error Messages

エラーメッセージ

Error messages (such as: message strings on exceptions like `ValueError`, or
messages shown to the user) should follow three guidelines:  
（`ValueError` などの例外にふくむメッセージや、ユーザーに表示するメッセージなどの）
エラーメッセージは次の三つのガイドラインに沿うようにします：

1.  The message needs to precisely match the actual error condition.  
    メッセージが実際のエラー条件に正確に一致すること

2.  Interpolated pieces need to always be clearly identifiable as such.  
    補間された項目がはっきりと識別できること

3.  They should allow simple automated processing (e.g. grepping).  
    単純な自動処理（grep など）でつかえること

```python
  Yes:
  if not 0 <= p <= 1:
    raise ValueError(f'Not a probability: {p!r}')

  try:
    os.rmdir(workdir)
  except OSError as error:
    logging.warning('Could not remove directory (reason: %r): %r',
                    error, workdir)
```

```python
  No:
  if p < 0 or p > 1:  # PROBLEM: also false for float('nan')!
                      # 問題点: float('nan') も偽になる！
    raise ValueError(f'Not a probability: {p!r}')

  try:
    os.rmdir(workdir)
  except OSError:
    # PROBLEM: Message makes an assumption that might not be true:
    # Deletion might have failed for some other reason, misleading
    # whoever has to debug this.
    # 問題点: メッセージでかならずしも真でない仮定をしている。削除は別の
    # 原因でも失敗するため、デバッグする人を誤誘導してしまう。
    logging.warning('Directory already was deleted: %s', workdir)

  try:
    os.rmdir(workdir)
  except OSError:
    # PROBLEM: The message is harder to grep for than necessary, and
    # not universally non-confusing for all possible values of `workdir`.
    # Imagine someone calling a library function with such code
    # using a name such as workdir = 'deleted'. The warning would read:
    # "The deleted directory could not be deleted."
    # 問題点: grep しづらいメッセージ。さらに `workdir` の取りうる
    # 値すべてに対して誤解なく読み取れない。このように書かれたライブラリーを
    # workdir = 'deleted' で呼びだすと警告はこうなる：
    # 「削除されたディレクトリーは削除できません。」
    logging.warning('The %s directory could not be deleted.', workdir)
```


<a id="files-and-sockets"></a>
<a id="files"></a>
### 3.11 Files, Sockets, and similar Stateful Resources

ファイルやソケットなど、状態をもつリソース

Explicitly close files and sockets when done with them. This rule naturally
extends to closeable resources that internally use sockets, such as database
connections, and also other resources that need to be closed down in a similar
fashion. To name only a few examples, this also includes
[mmap](https://docs.python.org/3/library/mmap.html) mappings,
[h5py File objects](https://docs.h5py.org/en/stable/high/file.html), and
[matplotlib.pyplot figure windows](https://matplotlib.org/2.1.0/api/_as_gen/matplotlib.pyplot.close.html).  
つかいおえたファイルやソケットは明示的に閉じます。
これはデータベース接続のように内部でソケットをつかうような「閉じうる」リソース全般、
同様の方法で使用後に閉じる必要があるリソースでも守ってください。わずかですが名前をあげると、
[mmap](https://docs.python.org/3/library/mmap.html) マッピング、
[h5py ファイルオブジェクト](https://docs.h5py.org/en/stable/high/file.html)、
[matplotlib.pyplot の figure ウィンドウ](https://matplotlib.org/2.1.0/api/_as_gen/matplotlib.pyplot.close.html)などです。

Leaving files, sockets or other such stateful objects open unnecessarily has
many downsides:  
ファイルやソケットその他の状態をもつオブジェクトを開いたまま放置するとたくさんの問題が生じます：

-   They may consume limited system resources, such as file descriptors. Code
    that deals with many such objects may exhaust those resources unnecessarily
    if they're not returned to the system promptly after use.  
    ファイルデスクリプターなど限られたシステムリソースを消費する。
    このようなオブジェクトを多数つかうコードが、
    使用後すぐにシステムに戻らないと不必要にリソースを枯渇させる恐れがある。
-   Holding files open may prevent other actions such as moving or deleting
    them, or unmounting a filesystem.  
    ファイルを開いたままにするとその移動や削除に加え、
    ファイルシステムのアンマウントといったその他操作をさまたげる。
-   Files and sockets that are shared throughout a program may inadvertently be
    read from or written to after logically being closed. If they are actually
    closed, attempts to read or write from them will raise exceptions, making
    the problem known sooner.  
    プログラム全域で共有されたファイルやソケットは閉じている途中で読み書きされる恐れがある。
    閉じおえたリソースへの読み書きは（これを早期検出するために）例外を送出させる。

Furthermore, while files and sockets (and some similarly behaving resources) are
automatically closed when the object is destructed, coupling the lifetime of the
object to the state of the resource is poor practice:  
加えて、ファイルやソケット（その他の似たふるまいをもつリソース）はオブジェクトの破棄時に閉じられますが、
オブジェクトの生存期間とリソースの状態をむすびつけるのは稚拙です。

-   There are no guarantees as to when the runtime will actually invoke the
    `__del__` method. Different Python implementations use different memory
    management techniques, such as delayed garbage collection, which may
    increase the object's lifetime arbitrarily and indefinitely.  
    `__del__` メソッドが呼ばれるタイミングは保証されていない。
    Python の実装が違えばメモリー管理も異なり、
    たとえば遅延ガーベッジコレクションでオブジェクトのライフは不測かつ無限に伸びることもある。
-   Unexpected references to the file, e.g. in globals or exception tracebacks,
    may keep it around longer than intended.  
    不測のファイルへの参照（たとえば大域変数や例外トレースバック内のキャプチャー）
    で意図したより長く生存することがある。

Relying on finalizers to do automatic cleanup that has observable side effects
has been rediscovered over and over again to lead to major problems, across many
decades and multiple languages (see e.g.
[this article](https://wiki.sei.cmu.edu/confluence/display/java/MET12-J.+Do+not+use+finalizers)
for Java).  
自動的な後片づけをファイナライザーに頼り、
これに観測可能な副作用があったため何度も繰りかえし再検出されて十年単位で複数の言語にまたがる大きな問題に発展した例もあります。（Java
の[この記事](https://wiki.sei.cmu.edu/confluence/display/java/MET12-J.+Do+not+use+finalizers)を参照）

The preferred way to manage files and similar resources is using the
[`with` statement](http://docs.python.org/reference/compound_stmts.html#the-with-statement):  
ファイルや類似のリソースは [`with` 文](http://docs.python.org/reference/compound_stmts.html#the-with-statement)で管理してください：

```python
with open("hello.txt") as hello_file:
    for line in hello_file:
        print(line)
```

For file-like objects that do not support the `with` statement, use
`contextlib.closing()`:  
`with` 文に対応していないファイル様のオブジェクトには `contextlib.closing()` をつかいます：

```python
import contextlib

with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print(line)
```

In rare cases where context-based resource management is infeasible, code
documentation must explain clearly how resource lifetime is managed.  
文脈でのリソース管理がつかえない場合は、コードの文書でそのライフ管理を明快に説明してください。


<a id="todo"></a>
### 3.12 TODO Comments

TODO コメント

Use `TODO` comments for code that is temporary, a short-term solution, or
good-enough but not perfect.  
`TODO` コメントは一時的なコード、短期的解決方法、十分だが完全ではない箇所につけます。

A `TODO` comment begins with the string `TODO` in all caps and a parenthesized
name, e-mail address, or other identifier
of the person or issue with the best context about the problem. This is followed
by an explanation of what there is to do.  
`TODO` コメントはすべて大文字で `TODO` と書き、丸カッコでくくって名前やメールアドレス、
個人もしくは問題をよくあらわす課題を付します。これにつづけて、なにをすべきかを説明します。

The purpose is to have a consistent `TODO` format that can be searched to find
out how to get more details. A `TODO` is not a commitment that the person
referenced will fix the problem. Thus when you create a
`TODO`, it is almost always your name
that is given.  
`TODO` の形式をそろえておけば、詳細情報の入手方法を探しやすくなります。
`TODO` は問題を修正する人の指定ではありません。
とどのつまり、あなたが作成する `TODO` にはあなたの名前を書きます。

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.  文字の繰り返しは "*" で。
# TODO(Zeke) Change this to use relations.  「関係」をつかうよう変える。
```

If your `TODO` is of the form "At a future date do something" make sure that you
either include a very specific date ("Fix by November 2009") or a very specific
event ("Remove this code when all clients can handle XML responses.").  
`TODO` が「いつかなにがしをする」という形にする場合、日時（「2009年11月までに修正」）
あるいは状況（「全クライアントが XML 応答を処理できるようになったら削除」）を特定してください。


<a id="imports-formatting"></a>
### 3.13 Imports formatting

インポートの整形

Imports should be on separate lines; there are
[exceptions for `typing` imports](#31912-imports-for-typing).  
インポートごとに行をわけます。
（[typing からのインポートは例外](#31912-imports-for-typing)です）

E.g.:  
すなわち：

```python
Yes: import os
     import sys
     from typing import Mapping, Sequence
```

```python
No:  import os, sys
```


Imports are always put at the top of the file, just after any module comments
and docstrings and before module globals and constants. Imports should be
grouped from most generic to least generic:  
インポートはファイルの先頭に書きます。インポートに先行するのはモジュールコメントと
docstring で、モジュールの大域変数と定数はインポートの後に配します。
インポートは一般的なものから順にグループ化します。

1.  Python future import statements. For example:  
    Python future インポート文からはじめます。

    ```python
    from __future__ import absolute_import
    from __future__ import division
    from __future__ import print_function
    ```

    See [above](#220-modern-python-from-__future__-imports) for more
    information about those.  
    詳しくは[前の節](#220-modern-python-from-__future__-imports)を参照してください。

2.  Python standard library imports. For example:  
    Python 標準ライブラリーインポート。

    ```python
    import sys
    ```

3.  [third-party](https://pypi.org/) module
    or package imports. For example:  
    [サードパーティ製](https://pypi.org/)のモジュールやパッケージのインポート。

    
    ```python
    import tensorflow as tf
    ```

4.  Code repository
    sub-package imports. For example:  
    コードリポジトリーに配されたサブパッケージのインポート。

    
    ```python
    from otherproject.ai import mind
    ```

5.  **Deprecated:** application-specific imports that are part of the same
    top level
    sub-package as this file. For example:  
    **廃止**: おなじトップレベル階層を共有するアプリケーション固有のインポート。

    
    ```python
    from myproject.backend.hgwells import time_machine
    ```

    You may find older Google Python Style code doing this, but it is no longer
    required. **New code is encouraged not to bother with this.** Simply treat
    application-specific sub-package imports the same as other sub-package
    imports.  
    古い Google Python Style Guide で勧めていましたが不要です。
    **新しいコードでは気にしないように。**
    アプリケーション固有サブパッケージのインポートは、ほかのサブパッケージのインポートと同様に扱ってください。

    
Within each grouping, imports should be sorted lexicographically, ignoring case,
according to each module's full package path (the `path` in `from path import
...`). Code may optionally place a blank line between import sections.  
各グループで、インポートは（大文字小文字を区別せず）辞書順にソートしてください。
またモジュールの完全パッケージパスにしたがうよう注意してください
（`from path import ...` なら `path` からの辞書順にします）。
インポートのグループごとに空行を入れてもかまいません。

```python
import collections
import queue
import sys

from absl import app
from absl import flags
import bs4
import cryptography
import tensorflow as tf

from book.genres import scifi
from myproject.backend import huxley
from myproject.backend.hgwells import time_machine
from myproject.backend.state_machine import main_loop
from otherproject.ai import body
from otherproject.ai import mind
from otherproject.ai import soul

# Older style code may have these imports down here instead:
# 古いスタイルガイドではこのあとで myproject をインポートするよう勧めていた：
#from myproject.backend.hgwells import time_machine
#from myproject.backend.state_machine import main_loop
```


<a id="statements"></a>
### 3.14 Statements

文

Generally only one statement per line.  
１行に１文を書きます。

However, you may put the result of a test on the same line as the test only if
the entire statement fits on one line. In particular, you can never do so with
`try`/`except` since the `try` and `except` can't both fit on the same line, and
you can only do so with an `if` if there is no `else`.  
ただ、１行にすべてが収まるなら検査と実行をまとめてもかまいません。
もちろん `try`/`except` は対象外です。というのも `try` と `except` は一行に書けないためです。
また `else` 節をもつ `if` 文も対象外です。

```python
Yes:

  if foo: bar(foo)
```

```python
No:

  if foo: bar(foo)
  else:   baz(foo)

  try:               bar(foo)
  except ValueError: baz(foo)

  try:
      bar(foo)
  except ValueError: baz(foo)
```


<a id="access-control"></a>
<a id="accessors"></a>
### 3.15 Accessors

アクセス用関数（アクセッサー）

If an accessor function would be trivial, you should use public variables
instead of accessor functions to avoid the extra cost of function calls in
Python. When more functionality is added you can use `property` to keep the
syntax consistent.  
アクセス用関数がささいな実装になるなら、アクセス用関数でなく公開変数をつかいます
（これにより Python の関数呼びだしコストをなくします）。
より多くの機能をくわえるなら `property` にして文法的な一貫性を維持します。

On the other hand, if access is more complex, or the cost of accessing the
variable is significant, you should use function calls (following the
[Naming](#316-naming) guidelines) such as `get_foo()` and `set_foo()`. If the
past behavior allowed access through a property, do not bind the new accessor
functions to the property. Any code still attempting to access the variable by
the old method should break visibly so they are made aware of the change in
complexity.  
一方でアクセスが複雑だったり変数のアクセスコストが無視できないなら、
（[名づけ](#316-naming)ガイドラインにしたがって） `get_foo()` や `set_foo()` のような関数呼びだしにします。
既存の実装がプロパティ経由のアクセスを公開していたなら、
あらたに追加するアクセス用関数をプロパティと結びつけてはいけません。
古い方法で変数をつかおうとするコードははっきりと壊れ、結果、複雑さによる変更に気づけます。


<a id="naming"></a>
### 3.16 Naming

名づけ

`module_name`, `package_name`, `ClassName`, `method_name`, `ExceptionName`,
`function_name`, `GLOBAL_CONSTANT_NAME`, `global_var_name`, `instance_var_name`,
`function_parameter_name`, `local_var_name`.


Function names, variable names, and filenames should be descriptive; eschew
abbreviation. In particular, do not use abbreviations that are ambiguous or
unfamiliar to readers outside your project, and do not abbreviate by deleting
letters within a word.  
関数名、変数名、ファイル名は叙述的にし、省略（頭字語）は避けます。
とくに頭字語はつかわないようにします。プロジェクト外の読者にはあいまいな上、不親切です。
また単語中の文字を削るのもやめましょう。

Always use a `.py` filename extension. Never use dashes.  
ファイル拡張子はつねに `.py` とします。ダッシュはつかいません。

<a id="names-to-avoid"></a>
#### 3.16.1 Names to Avoid 

避けるべき名前

-   single character names, except for specifically allowed cases:  
    １文字の名前。ただし以下特例を除く：

    -   counters or iterators (e.g. `i`, `j`, `k`, `v`, et al.)  
        カウンターや列挙名
    -   `e` as an exception identifier in `try/except` statements.  
        `try/except` 文中での例外 `e`
    -   `f` as a file handle in `with` statements  
        `with` 文中でのファイルハンドル `f`

    Please be mindful not to abuse single-character naming. Generally speaking,
    descriptiveness should be proportional to the name's scope of visibility.
    For example, `i` might be a fine name for 5-line code block but within
    multiple nested scopes, it is likely too vague.  
    一文字名を誤用しないよう配慮してください。一般的に、
    名前の可視スコープに応じて名前を詳しくします。たとえば５行のコードブロック内での
    `i` はよくても、多段の入れ子スコープではあいまいすぎるでしょう。

-   dashes (`-`) in any package/module name  
    パッケージ・モジュール名にふくまれるダッシュ（`-`）

-   `__double_leading_and_trailing_underscore__` names (reserved by Python)  
    先頭と末尾にふたつのアンダースコア `_` をつけた名前（Python の予約語）

-   offensive terms  
    罵倒語

-   names that needlessly include the type of the variable (for example:
    `id_to_name_dict`)  
    不要な型情報をふくむ名前（たとえば `id_to_name_dict`）

<a id="naming-conventions"></a>
#### 3.16.2 Naming Conventions

命名規則

-   "Internal" means internal to a module, or protected or private within a
    class.  
    「内部用」はモジュール内のみ、あるいはクラス内の protect、 private。

-   Prepending a single underscore (`_`) has some support for protecting module
    variables and functions (linters will flag protected member access).  
    アンダースコアを一つ前置することで、モジュール変数、
    モジュール関数としてある種の保護が受けられる（lint ツールがプロテクトメンバー用にマークしてくれる）

-   Prepending a double underscore (`__` aka "dunder") to an instance variable
    or method effectively makes the variable or method private to its class
    (using name mangling); we discourage its use as it impacts readability and
    testability, and isn't *really* private. Prefer a single underscore.  
    アンダースコアをふたつインスタンス変数やメソッドに前置する（“dunder” ともいう `__`）
    と、（名前修飾として）変数やメソッドがそのクラスの private という意味になる。
    しかしつかわないように。読みづらく、テストしづらく、そして *実際のところ* private
    でもない。アンダースコアはひとつにする。

-   Place related classes and top-level functions together in a
    module.
    Unlike Java, there is no need to limit yourself to one class per module.  
    関連するクラス群とトップレベル関数群はおなじモジュールに入れる。
    Java とは違い１モジュール１クラスの制限はない。

-   Use CapWords for class names, but lower\_with\_under.py for module names.
    Although there are some old modules named CapWords.py, this is now
    discouraged because it's confusing when the module happens to be named after
    a class. ("wait -- did I write `import StringIO` or `from StringIO import
    StringIO`?")  
    クラス名にはキャメルケースをつかい、モジュール名にはスネークケースをつかう。
    キャメルケース名の古いモジュールがあるが現在は非推奨。
    クラス名にちなんで名づけられたモジュール名はとくにまぎらわしい。
    （「あれ、 `import StringIO` としたっけ、それとも `from StringIO import StringIO`？」）

-   Underscores may appear in *unittest* method names starting with `test` to
    separate logical components of the name, even if those components use
    CapWords. One possible pattern is `test<MethodUnderTest>_<state>`; for
    example `testPop_EmptyStack` is okay. There is no One Correct Way to name
    test methods.  
    `test` を前置する *ユニットテスト* のメソッド名にアンダースコアをつかうことがある。
    これでテスト名にふくまれる論理コンポーネント（これら名前がキャメルケースで識別できるとしても）
    を分割する。 `test<MethodUnderTest>_<state> `といったパターン、たとえば
    `testPop_EmptyStack` はよい。テストメソッド名には「唯一絶対の正解」はない。

<a id="file-naming"></a>
#### 3.16.3 File Naming

ファイル名

Python filenames must have a `.py` extension and must not contain dashes (`-`).
This allows them to be imported and unittested. If you want an executable to be
accessible without the extension, use a symbolic link or a simple bash wrapper
containing `exec "$0.py" "$@"`.  
Python のファイル名は `.py` 拡張子をつけダッシュ（`-`）を含まないものとします。
こうすることでインポートでき、ユニットテストもできます。拡張子なしの実行形式は、
シンボリックリンクを張るか、 `exec "$0.py" "$@"` と定義した bash ラッパーをつかいます。

<a id="guidelines-derived-from-guidos-recommendations"></a>
#### 3.16.4 Guidelines derived from [Guido](https://en.wikipedia.org/wiki/Guido_van_Rossum)'s Recommendations

Guido のおすすめから作成したガイドライン

<table rules="all" border="1" summary="Guidelines from Guido's Recommendations"
       cellspacing="2" cellpadding="2">

  <tr>
    <th>Type （種別）</th>
    <th>Public （公開）</th>
    <th>Internal （内部用）</th>
  </tr>

  <tr>
    <td>Packages</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Modules</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code></td>
  </tr>

  <tr>
    <td>Classes</td>
    <td><code>CapWords</code></td>
    <td><code>_CapWords</code></td>
  </tr>

  <tr>
    <td>Exceptions</td>
    <td><code>CapWords</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Functions</td>
    <td><code>lower_with_under()</code></td>
    <td><code>_lower_with_under()</code></td>
  </tr>

  <tr>
    <td>Global/Class Constants</td>
    <td><code>CAPS_WITH_UNDER</code></td>
    <td><code>_CAPS_WITH_UNDER</code></td>
  </tr>

  <tr>
    <td>Global/Class Variables</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code></td>
  </tr>

  <tr>
    <td>Instance Variables</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code> (protected)</td>
  </tr>

  <tr>
    <td>Method Names</td>
    <td><code>lower_with_under()</code></td>
    <td><code>_lower_with_under()</code> (protected)</td>
  </tr>

  <tr>
    <td>Function/Method Parameters</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Local Variables</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

</table>

<a id="math-notation"></a>
#### 3.16.5 Mathematical Notation

数学的記法

For mathematically heavy code, short variable names that would otherwise violate
the style guide are preferred when they match established notation in a
reference paper or algorithm. When doing so, reference the source of all naming
conventions in a comment or docstring or, if the source is not accessible,
clearly document the naming conventions. Prefer PEP8-compliant
`descriptive_names` for public APIs, which are much more likely to be
encountered out of context.  
数式重視のコードではスタイルガイドに反してでも、
論文やアルゴリズムで確立された記法にそって短い変数名をつかってください。
この場合、参照したソースすべての名前規約をコメントや docstring に書くか、
ソースが非公開なら命名規則を明瞭に文書化します。
公開 API は `descriptive_names` のような PEP8 形式の名前にしてください。
（数式の文脈を離れたらこれがよくある形式でしょう）


<a id="main"></a>
### 3.17 Main

main 関数

In Python, `pydoc` as well as unit tests require modules to be importable. If a
file is meant to be used as an executable, its main functionality should be in a
`main()` function, and your code should always check `if __name__ == '__main__'`
before executing your main program, so that it is not executed when the module
is imported.  
Python では `pydoc` やユニットテストをつかうために、モジュールはインポートできなくてはなりません。
ファイルを実行可能にするなら、その主要機能は `main()` 関数に書きます。
そしてモジュールのインポート時には動作しないよう` if __name__ == '__main__'`
を確認してから主プログラムを実行します。

When using [absl](https://github.com/abseil/abseil-py), use `app.run`:  
[absl](https://github.com/abseil/abseil-py) をつかっているなら `app.run` で起動します：

```python
from absl import app
...

def main(argv: Sequence[str]):
    # process non-flag arguments
    ...

if __name__ == '__main__':
    app.run(main)
```

Otherwise, use:  
そうでなければこう書きます：

```python
def main():
    ...

if __name__ == '__main__':
    main()
```

All code at the top level will be executed when the module is imported. Be
careful not to call functions, create objects, or perform other operations that
should not be executed when the file is being `pydoc`ed.  
トップレベルのコードはモジュールがインポートされたときに実行されます。
ファイルを `pydoc` するときに不要な、関数の呼びだしやオブジェクトの作成、
その他の操作を実行しないよう注意してください。


<a id="function-length"></a>
### 3.18 Function length

関数の長さ

Prefer small and focused functions.  
小さく対象をしぼった関数を書きます。

We recognize that long functions are sometimes appropriate, so no hard limit is
placed on function length. If a function exceeds about 40 lines, think about
whether it can be broken up without harming the structure of the program.  
長い関数が適切な場合もあるので関数の長さは物理的に制限しません。
ただ 40 行を超えるようなら、プログラムの構造を害することなく分割できないか考えてみてください。

Even if your long function works perfectly now, someone modifying it in a few
months may add new behavior. This could result in bugs that are hard to find.
Keeping your functions short and simple makes it easier for other people to read
and modify your code.  
現時点で長い関数が完璧に動いているとしても、数か月後に誰かが書きかえて別のふるまいを追加します。
これは探しにくいバグを生みます。関数が短く簡潔ならば、ほかの人も読みやすく書きかえやすくなります。

You could find long and complicated functions when working with some
code. Do not be intimidated by modifying existing code: if working with such a
function proves to be difficult, you find that errors are hard to debug, or you
want to use a piece of it in several different contexts, consider breaking up
the function into smaller and more manageable pieces.  
コードを書いていれば長く複雑な関数に行きつくことがあるでしょう。
既存コードを書きかえることを恐れないでください。そういった関数の利用が難しいとわかったら、
エラーをデバッグしづらいなら、違った環境で一部分だけつかってみたいなら、
関数を小わけにして扱いやすくしましょう。


<a id="type-annotations"></a>
### 3.19 Type Annotations

型注釈

<a id="typing-general"></a>
#### 3.19.1 General Rules

総則

*   Familiarize yourself with
    [PEP-484](https://www.python.org/dev/peps/pep-0484/).  
    [PEP-484](https://www.python.org/dev/peps/pep-0484/) に慣れ親しむ。
*   In methods, only annotate `self`, or `cls` if it is necessary for proper
    type information. e.g., `@classmethod def create(cls: Type[T]) -> T: return
    cls()`  
    メソッドでは、適切な型情報が必要なときに限り `self` や `cls` を注釈する。
    たとえば `@classmethod def create(cls: Type[T]) -> T: return cls()`
*   If any other variable or a returned type should not be expressed, use `Any`.  
    型情報を与えない変数や戻り値には `Any` をつかう。
*   You are not required to annotate all the functions in a module.  
    モジュール内関数すべてを注釈しなくてよい。
    -   At least annotate your public APIs.  
        最低限、公開する API は注釈する。
    -   Use judgment to get to a good balance between safety and clarity on the
        one hand, and flexibility on the other.  
        一方を安全性と明快さ、他方を柔軟性としてうまくバランスを取る。
    -   Annotate code that is prone to type-related errors (previous bugs or
        complexity).  
        型に起因する問題を起こしたコードは注釈する（以前のバグや複雑なもの）。
    -   Annotate code that is hard to understand.  
        理解しづらいコードは注釈する。
    -   Annotate code as it becomes stable from a types perspective. In many
        cases, you can annotate all the functions in mature code without losing
        too much flexibility.  
        型の観点で安定したコードは注釈する。
        こなれたコード中のほとんどの関数は柔軟性をうしなわずに注釈できる。

<a id="typing-line-breaking"></a>
#### 3.19.2 Line Breaking

改行

Try to follow the existing [indentation](#34-indentation) rules.  
既存のインデントルールにしたがってください。

After annotating, many function signatures will become "one parameter per line".  
注釈すると、たいていの関数署名は「１引数１行」になります。

```python
def my_method(self,
              first_var: int,
              second_var: Foo,
              third_var: Optional[Bar]) -> int:
  ...
```

Always prefer breaking between variables, and not, for example, between variable
names and type annotations. However, if everything fits on the same line, go for
it.  
変数ごとに改行してください。つまり変数名と型注釈の途中では改行しないでください。
他方、一行におさまるならそうしてください。

```python
def my_method(self, first_var: int) -> int:
  ...
```

If the combination of the function name, the last parameter, and the return type
is too long, indent by 4 in a new line.  
関数名、最後の引数、戻り値型の組み合わせで長くなるなら行を変えて４字下げしてください。

```python
def my_method(
    self, first_var: int) -> Tuple[MyLongType1, MyLongType1]:
  ...
```

When the return type does not fit on the same line as the last parameter, the
preferred way is to indent the parameters by 4 on a new line and align the
closing parenthesis with the `def`.  
戻り値型と最後の引数が同一行に収まらないなら、望ましいのは行を変えて４字下げして引数を並べ、
閉じ括弧を `def` に揃えます。

```python
Yes:
def my_method(
    self, other_arg: Optional[MyLongType]
) -> Dict[OtherLongType, MyLongType]:
  ...
```

`pylint` allows you to move the closing parenthesis to a new line and align
with the opening one, but this is less readable.  
pylint は改行後の閉じ括弧位置を開き括弧位置にあわせてよいとしています。
しかし少々読みにくくなります。

```python
No:
def my_method(self,
              other_arg: Optional[MyLongType]
             ) -> Dict[OtherLongType, MyLongType]:
  ...
```

As in the examples above, prefer not to break types. However, sometimes they are
too long to be on a single line (try to keep sub-types unbroken).  
上に示した例のとおり、型の途中で改行しないよう心がけてください。
しかし一行に収まらないほど長くなることはあります。（それでも内側の型が分離しないよう工夫してください）

```python
def my_method(
    self,
    first_var: Tuple[List[MyLongType1],
                     List[MyLongType2]],
    second_var: List[Dict[
        MyLongType3, MyLongType4]]) -> None:
  ...
```

If a single name and type is too long, consider using an
[alias](#3196-type-aliases) for the type. The last resort is to break after the
colon and indent by 4.  
ひとつの名前と型でも長くなるなら、型の[エイリアス](#3196-type-aliases)も検討してください。
最後の手段はコロンの後ろで改行して４字下げすることです。

```python
Yes:
def my_function(
    long_variable_name:
        long_module_name.LongTypeName,
) -> None:
  ...
```

```python
No:
def my_function(
    long_variable_name: long_module_name.
        LongTypeName,
) -> None:
  ...
```

<a id="forward-declarations"></a>
#### 3.19.3 Forward Declarations

前方宣言

If you need to use a class name from the same module that is not yet defined --
for example, if you need the class inside the class declaration, or if you use a
class that is defined below -- use a string for the class name.  
おなじモジュール内でまだ定義を終えていないクラスが必要なら、
（たとえばクラスの定義中にそのクラスが必要になるとか、あとで定義するクラスをつかうときなど）
クラス名の文字列をつかいます。

```python
class MyClass:

  def __init__(self,
               stack: List["MyClass"]) -> None:
```

<a id="typing-default-values"></a>
#### 3.19.4 Default Values

規定値

As per
[PEP-008](https://www.python.org/dev/peps/pep-0008/#other-recommendations), use
spaces around the `=` _only_ for arguments that have both a type annotation and
a default value.  
[PEP-008](https://www.python.org/dev/peps/pep-0008/#other-recommendations) にしたがい、
型注釈と規定値の両方をもつ引数の `=` の周囲には必ずスペースを置きます。
（訳注：スペースを置かなければいけないし、スペース以外の文字を入れてもいけない。改行もダメ）

```python
Yes:
def func(a: int = 0) -> int:
  ...
```

```python
No:
def func(a:int=0) -> int:
  ...
```

<a id="none-type"></a>
#### 3.19.5 NoneType

NoneType

In the Python type system, `NoneType` is a "first class" type, and for typing
purposes, `None` is an alias for `NoneType`. If an argument can be `None`, it
has to be declared! You can use `Union`, but if there is only one other type,
use `Optional`.  
Python の型システムにおいて `NoneType` は「一級」の型で、 `None` はその別名です。
引数が `None` になりうるなら、そのように宣言しましょう！ `Union` もつかえますが、
それ以外の型がただひとつなら `Optional` をつかいます。

Use explicit `Optional` instead of implicit `Optional`. Earlier versions of PEP
484 allowed `a: str = None` to be interpreted as `a: Optional[str] = None`, but
that is no longer the preferred behavior.  
暗黙的にでなく明示的に `Optional` を指定します。 PEP 484 の以前の版では
`a: str = None` を `a: Optional[str] = None` と解釈できるされていましたが、
望ましいふるまいでなくなりました。

```python
Yes:
def func(a: Optional[str], b: Optional[str] = None) -> str:
  ...
def multiple_nullable_union(a: Union[None, str, int]) -> str:
  ...
```

```python
No:
def nullable_union(a: Union[None, str]) -> str:
  ...
def implicit_optional(a: str = None) -> str:
  ...
```

<a id="typing-aliases"></a>
<a id="type-aliases"></a>
#### 3.19.6 Type Aliases

型エイリアス

You can declare aliases of complex types. The name of an alias should be
CapWorded. If the alias is used only in this module, it should be \_Private.  
複雑な型に別名（エイリアス）をつけられます。エイリアス名はキャメルケースにしてください。
エイリアスをモジュール内だけでつかうときは必ず `_` を前置します。

For example, if the name of the module together with the name of the type is too
long:  
たとえばモジュール名を付した型名がとても長くなるときに：

```python
_ShortName = module_with_long_name.TypeWithLongName
ComplexMap = Mapping[str, List[Tuple[int, int]]]
```

Other examples are complex nested types and multiple return variables from a
function (as a tuple).  
ほかにも複雑な入れ子の型や複数の値を（タプルとして）返す関数などがあります。

<a id="typing-ignore"></a>
#### 3.19.7 Ignoring Types

型情報の放棄

You can disable type checking on a line with the special comment `# type:
ignore`.  
`# type: ignore` という特殊コメントで、行ごとに型チェックを停止できます。

`pytype` has a disable option for specific errors (similar to lint):  
`pytype` には特定のエラーを無視する（lint に似た）オプションがあります。

```python
# pytype: disable=attribute-error
```

<a id="typing-variables"></a>
#### 3.19.8 Typing Variables

型変数

If an internal variable has a type that is hard or impossible to infer, you can
specify its type in a couple ways.  
型推論が難しかったり推論できない変数について、その型を指定するふたつの方法があります。

<a id="type-comments"></a>
[*Type Comments:*](#type-comments) 型コメント
:   Use a `# type:` comment on the end of the line  
    行末尾に `# type:` コメントを付します。

```python
a = SomeUndecoratedFunction()  # type: Foo
```

<a id="annotated-assignments"></a>
[*Annotated Assignments*](#annotated-assignments) 注釈つき代入
:   Use a colon and type between the variable name and value, as with function
    arguments.  
    関数引数のように、変数名と値の間にコロンと型をはさみます。

```python
a: Foo = SomeUndecoratedFunction()
```

<a id="typing-tuples"></a>
#### 3.19.9 Tuples vs Lists

タプルかリストか

Typed lists can only contain objects of a single type. Typed tuples can either
have a single repeated type or a set number of elements with different types.
The latter is commonly used as the return type from a function.  
型つきのリストは特定の型のオブジェクトだけを要素にもちます。
型つきのタプルは特定の型の繰り返しとしても、異なる型の要素の組み合わせとしてもつかえます。
後者は関数の戻り値の型としてよくつかわれます。

```python
a = [1, 2, 3]  # type: List[int]
b = (1, 2, 3)  # type: Tuple[int, ...]
c = (1, "2", 3.5)  # type: Tuple[int, str, float]
```

<a id="typevars"></a>
#### 3.19.10 TypeVars

型変数

The Python type system has
[generics](https://www.python.org/dev/peps/pep-0484/#generics). The factory
function `TypeVar` is a common way to use them.  
Python の型システムは[ジェネリックス](https://www.python.org/dev/peps/pep-0484/#generics)があります。
このために生成関数 `TypeVar` がよくつかわれます。

Example:  
たとえば：

```python
from typing import List, TypeVar
T = TypeVar("T")
...
def next(l: List[T]) -> T:
  return l.pop()
```

A TypeVar can be constrained:  
制約を与えることもできます：

```python
AddableType = TypeVar("AddableType", int, float, str)
def add(a: AddableType, b: AddableType) -> AddableType:
  return a + b
```

A common predefined type variable in the `typing` module is `AnyStr`. Use it for
multiple annotations that can be `bytes` or `unicode` and must all be the same
type.  
`typing` モジュールで定義されている、おなじみの型変数 `AnyStr` があります。
`bytes` もしくは `unicode` で、いずれか一方であるという複数個所を注釈するためにつかいます。

```python
from typing import AnyStr
def check_length(x: AnyStr) -> AnyStr:
  if len(x) <= 42:
    return x
  raise ValueError()
```

<a id="typing-strings"></a>
#### 3.19.11 String types

文字列型

The proper type for annotating strings depends on what versions of Python the
code is intended for.  
文字列の注釈につかう型は、プログラムがどのバージョンの Python 向けかで異なります。

Prefer to use `str`, though `Text` is also acceptable. Be consistent in using
one or the other. For code that deals with binary data, use `bytes`. For Python
2 compatible code that processes text data (`str` or `unicode` in Python 2,
`str` in Python 3), use `Text`.  
（`Text` もつかえますが） `str` をつかいます。どちらか一方で統一してください。
バイナリーデータを処理するプログラムでは `bytes` をつかいます。
Python 2 互換のテキスト（Python 2 では `str` と `unicode`、 Python 3 では `str`）
処理では `Text` をつかいます。

```python
def deals_with_text_data_in_py3(x: str) -> str:
  ...
def deals_with_binary_data(x: bytes) -> bytes:
  ...
def py2_compatible_text_data_processor(x: Text) -> Text:
  ...
```

In some uncommon Python 2 compatibility cases, `str` may make sense instead of
`Text`, typically to aid compatibility when the return types aren't the same
between Python 2 and Python 3. Never use `unicode` as it doesn't exist in
Python 3. The reason this discrepancy exists is because `str` means something
different in Python 2 than in Python 3.  
Python 2 互換問題として、 `Text` でなく `str` がよい場合があります。
とくに Python 2 と Python 3 とで戻り値の型がおなじでなく、なお互換対応するときです。
Python 3 にはない `unicode` は決してつかいません。 Python 2 と 3 で `str`
の意味が微妙に異なるため、このような不一致が生じます。

No:  
ダメ：

```python
def py2_code(x: str) -> unicode:
  ...
```

If the type can be either bytes or text, use `Union`, with the appropriate text
type.  
bytes と text のいずれも受け入れる型なら、適切な text 型を指定して `Union` をつかいます。

```python
from typing import Text, Union
...
def py3_only(x: Union[bytes, str]) -> Union[bytes, str]:
  ...
def py2_compatible(x: Union[bytes, Text]) -> Union[bytes, Text]:
  ...
```

If all the string types of a function are always the same, for example if the
return type is the same as the argument type in the code above, use
[AnyStr](#31910-typevars).  
関数の文字列型がすべておなじ、
たとえば上のコードで戻り値型が引数の型と一致するようなときは
[AnyStr](#31910-typevars) をつかいます。

<a id="typing-imports"></a>
#### 3.19.12 Imports For Typing

型づけのためのインポート

For classes from the `typing` module, always import the class itself. You are
explicitly allowed to import multiple specific classes on one line from the
`typing` module. Ex:  
`typing` モジュール定義のクラスはクラス名をインポートします。
`typing` モジュールのクラスは一行でまとめてインポートしてよいです。たとえば：

```python
from typing import Any, Dict, Optional
```

Given that this way of importing from `typing` adds items to the local
namespace, any names in `typing` should be treated similarly to keywords, and
not be defined in your Python code, typed or not. If there is a collision
between a type and an existing name in a module, import it using `import x as
y`.  
このような `typing` モジュールからのインポートはローカルの名前空間に要素を追加します。
`typing` モジュール内のすべての名前はキーワードのようにつかいます。型づけの有無にかかわらず、
Python コードで定義されたものとはみなしません。型とモジュール内の既存の名前が衝突した場合、
これを `import x as y` のようにインポートします。

```python
from typing import Any as AnyType
```

<a id="typing-conditional-imports"></a>
#### 3.19.13 Conditional Imports

条件つきインポート

Use conditional imports only in exceptional cases where the additional imports
needed for type checking must be avoided at runtime. This pattern is
discouraged; alternatives such as refactoring the code to allow top level
imports should be preferred.  
条件つきインポートは次のみを特例として認めます：　型チェックのために追加インポートが必要で、
実行時にはインポートしない場合。ただしこの方法は避け、
トップレベルのインポートだけになるようコードをリファクタリングするといった別手段を検討してください。

Imports that are needed only for type annotations can be placed within an `if
TYPE_CHECKING:` block.  
型注釈のためだけに必要となるインポートは `if TYPE_CHECKING:` ブロックに書きます。

-   Conditionally imported types need to be referenced as strings, to be forward
    compatible with Python 3.6 where the annotation expressions are actually
    evaluated.  
    Python 3.6 以前との互換性のため、条件つきインポートする型は文字列で参照する。
    （Python 3.6 以前では注釈式が実際に評価される）
-   Only entities that are used solely for typing should be defined here; this
    includes aliases. Otherwise it will be a runtime error, as the module will
    not be imported at runtime.  
    型づけのためにだけつかう対象をここに定義する（エイリアスも含む）。
    そうしなければ実行時エラーとなり、実行時にモジュールがインポートされなくなる。
-   The block should be right after all the normal imports.  
    このブロックはすべての通常のインポート文の直後に配置する。
-   There should be no empty lines in the typing imports list.  
    typing インポート文のリスト内に空行をいれない。
-   Sort this list as if it were a regular imports list.  
    ふつうのインポート文のリストのようにこのリストもソートする。
```python
import typing
if typing.TYPE_CHECKING:
  import sketch
def f(x: "sketch.Sketch"): ...
```

<a id="typing-circular-deps"></a>
#### 3.19.14 Circular Dependencies

循環依存

Circular dependencies that are caused by typing are code smells. Such code is a
good candidate for refactoring. Although technically it is possible to keep
circular dependencies, various build systems will not let you do so
because each module has to depend on the other.  
型づけのために循環依存が生じたとしたらコードが臭っています（訳注：
「コードの臭い」は書籍『リファクタリング』マーチン・ファウラーに出てくる、
コードを改善したほうがよいと感じられるポイント）。
これはリファクタリングのよい候補といえます。技術的には循環依存を残しておけますが、
いくつものビルドシステムはこれを許さないように、
つまりあるモジュールは他のモジュールにだけ依存するようにしています。

Replace modules that create circular dependency imports with `Any`. Set an
[alias](#3196-type-aliases) with a meaningful name, and use the real type name from
this module (any attribute of Any is Any). Alias definitions should be separated
from the last import by one line.  
循環依存を引きおこすモジュールを `Any` のインポートに置きかえます。
意味のある名前で別名（[エイリアス](#3196-type-aliases)）を定義し、
このモジュールから取りこむ実名にします（`Any` のあらゆる属性の型は `Any` です）。
別名定義はただ一行、最後のインポートだけで隔離できます。

```python
from typing import Any

some_mod = Any  # some_mod.py imports this module.
...

def my_method(self, var: "some_mod.SomeType") -> None:
  ...
```

<a id="generics"></a>
#### 3.19.15 Generics

ジェネリクス

When annotating, prefer to specify type parameters for generic types; otherwise,
[the generics' parameters will be assumed to be `Any`](https://www.python.org/dev/peps/pep-0484/#the-any-type).  
注釈するときは型パラメーターを特定してください。
特定していない[ジェネリクスのパラメーターは Any](https://www.python.org/dev/peps/pep-0484/#the-any-type)
と解釈されます。

```python
def get_names(employee_ids: List[int]) -> Dict[int, Any]:
  ...
```

```python
# These are both interpreted as get_names(employee_ids: List[Any]) -> Dict[Any, Any]
# 以下いずれも get_names(employee_ids: List[Any]) -> Dict[Any, Any] 扱い。
def get_names(employee_ids: list) -> Dict:
  ...

def get_names(employee_ids: List) -> Dict:
  ...
```

If the best type parameter for a generic is `Any`, make it explicit, but
remember that in many cases [`TypeVar`](#31910-typevars) might be more
appropriate:  
最良の型パラメーターが `Any` ならこれを明示します。
しかしまず [`TypeVar`](#31910-typevars) がより適切であると忘れないでください：

```python
def get_names(employee_ids: List[Any]) -> Dict[Any, str]:
  """Returns a mapping from employee ID to employee name for given IDs."""
```

```python
T = TypeVar('T')
def get_names(employee_ids: List[T]) -> Dict[T, str]:
  """Returns a mapping from employee ID to employee name for given IDs."""
```


<a id="consistency"></a>
## 4 Parting Words

むすび

*BE CONSISTENT*.  
*一貫性を*。

If you're editing code, take a few minutes to look at the code around you and
determine its style. If they use spaces around all their arithmetic operators,
you should too. If their comments have little boxes of hash marks around them,
make your comments have little boxes of hash marks around them too.  
コードを書きかえはじめる前に、その周辺を読んでスタイルを把握する時間を取ってください。
数値演算子の前後に空白がはいっているならそれに倣う。コメントを # で箱囲いしているなら、
同じように囲う。

The point of having style guidelines is to have a common vocabulary of coding so
people can concentrate on what you're saying rather than on how you're saying
it. We present global style rules here so people know the vocabulary, but local
style is also important. If code you add to a file looks drastically different
from the existing code around it, it throws readers out of their rhythm when
they go to read it. Avoid this.  
スタイルのガイドラインを持つ意義は、コーディングの語彙を共有することにあります。
これにより、言いかたでなくその内容に集中できます。このスタイル規則で共通の語彙ができました。
しかしすでにある書式もまた大事です。
あなたの書くコードが周辺とあまりに違うと読み手のリズムを乱してしまいます。そうならないように。
