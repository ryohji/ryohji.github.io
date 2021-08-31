# Google Python Style Guide

Table of Contents
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

## 1 Background

背景

Python is the main dynamic language used at Google.
This style guide is a list of dos and don’ts for Python programs.  
Python は Google の主要な動的言語です。
このスタイルガイドは Python プログラムですべきこと、してはいけないことを挙げたものです。


To help you format code correctly, we’ve created a
[settings file for Vim](https://google.github.io/styleguide/google_python_style.vim).
For Emacs, the default settings should be fine.  
コードをただしく整形するために [Vim 設定ファイル](https://google.github.io/styleguide/google_python_style.vim)を用意しました。
Emacs は初期設定で充分です。


Many teams use the [yapf](https://github.com/google/yapf/)
auto-formatter to avoid arguing over formatting.  
たいていのチームは yapf 自動整形ツールをつかって、整形で紛糾しないようにしています。


## 2 Python Language Rules

Python 言語に関わる規則

### 2.1 Lint

Run `pylint` over your code using this [pylintrc](https://google.github.io/styleguide/pylintrc).  
コード全体に [pylintrc](https://google.github.io/styleguide/pylintrc) の設定で `pylint` をかけます。

#### 2.1.1 Definition

定義

`pylint` is a tool for finding bugs and style problems in Python source code.
It finds problems that are typically caught by a compiler for less dynamic
languages like C and C++. Because of the dynamic nature of Python, some
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

`pylint` isn’t perfect. To take advantage of it, sometimes we’ll need to write
around it, suppress its warnings or fix it.  
`pylint` は完璧ではありません。
結果を得るためには書き加えたり抑止したり修正したりといった作業が必要です。

#### 2.1.4 Decision

取り決め

Make sure you run pylint on your code.  
かならず `pylint` をコードに適用してください。

Suppress warnings if they are inappropriate so that other issues are not hidden.
To suppress warnings, you can set a line-level comment:  
（そのためにほかの問題が消えなくなるような）不適切な警告は抑止します。
抑止は行単位のコメントで設定します：

```python
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
```

`pylint` warnings are each identified by symbolic name (`empty-docstring`)
Google-specific warnings start with `g-`.  
`pylint` 警告には固有のシンボル名があります（`empty-docstring` など）。
Google が定義した警告名は `g-` ではじまります。

If the reason for the suppression is not clear from the symbolic name, add an
explanation.  
抑止した理由がシンボル名から判然としないなら説明をくわえてください。

Suppressing in this way has the advantage that we can easily search for
suppressions and revisit them.  
このように抑止していくことで抑止した箇所を探しやすく、そして再チェックしやすくなります。

You can get a list of `pylint` warnings by doing:  
`pylint` 警告はつぎのようにして一覧できます：

```
pylint --list-msgs
```

To get more information on a particular message, use:  
より詳細に知りたい場合は次のようにします：

```
pylint --help-msg=C6409
```

Prefer `pylint: disable` to the deprecated older form `pylint: disable-msg`.  
`pylint: disable` で抑止してください。（古い `pylint: disable-msg` はつかいません）

Unused argument warnings can be suppressed by deleting the variables at the
beginning of the function. Always include a comment explaining why you are
deleting it. “Unused.” is sufficient. For example:  
未使用の引数の警告には関数冒頭で削除して対応してください。
削除理由もあわせてコメントしてください。「未使用」で充分です。たとえば：

```python
def viking_cafe_order(spam: str, beans: str, eggs: Optional[str] = None) -> str:
    del beans, eggs  # Unused by vikings.
    return spam + spam + spam
```

Other common forms of suppressing this warning include using ‘`_`’ as the
identifier for the unused argument or prefixing the argument name with
‘`unused_`’, or assigning them to ‘`_`’. These forms are allowed but no longer
encouraged. These break callers that pass arguments by name and do not enforce
that the arguments are actually unused.  
警告に対応するその他の方法もあります。変数名を ‘`_`’ にしたり引数名に ‘`unused_`’
を前置したり ‘`_`’ に代入するなどです。
これら方法をつかってもかまいませんがお勧めしません。
これらの方法は引数名を指定した呼びだしを壊しますし、引数がつかわれないことも保証できません。

### 2.2 Imports

インポート

Use `import` statements for packages and modules only, not for individual
classes or functions. Imports from the
[typing module](https://google.github.io/styleguide/pyguide.html#typing-imports),
[typing_extensions module](https://github.com/python/typing/tree/master/typing_extensions),
and the [six.moves module](https://six.readthedocs.io/#module-six.moves) are
exempt from this rule.  
`import` 文はパッケージとモジュールのためだけに使い、個々のクラスや関数を取り込むためには使いません。
ただし [typing モジュール](https://google.github.io/styleguide/pyguide.html#typing-imports)、
[typing_extensions モジュール](https://github.com/python/typing/tree/master/typing_extensions)、
そして [six.moves モジュール](https://six.readthedocs.io/#module-six.moves)は例外とします。

#### 2.2.1 Definition

定義

Reusability mechanism for sharing code from one module to another.  
あるモジュールから別モジュールへコードを共有する、再利用の仕組みです。

#### 2.2.2 Pros

利点

The namespace management convention is simple. The source of each identifier
is indicated in a consistent way; `x.Obj` says that object `Obj` is defined in
module `x`.  
名前空間を管理する規約は単純です。各識別子に対応するソースは一貫した方法で示されます。
たとえば `x.Obj` はオブジェクト `Obj` はモジュール `x` で定義されているといった具合です。

#### 2.2.3 Cons

欠点

Module names can still collide. Some module names are inconveniently long.  
モジュール名が衝突することがあります。つかいづらいほど長い名前のモジュールもあります。

#### 2.2.4 Decision

取り決め

* Use `import x` for importing packages and modules.  
`import x` はパッケージあるいはモジュールのインポートに使います。
* Use `from x import y` where `x` is the package prefix and `y` is the module
name with no prefix.  
`from x import y` は前置されるパッケージ名 `x` に対しモジュール名 `y` を前置なしで
参照するために使います。
* Use `from x import y as z` if two modules named `y` are to be imported or if
`y` is an inconveniently long name.  
`from x import y as z` は同名のモジュール `y` をインポートするとき、あるいは
`y` が不必要に長いときに使います。
* Use `import y as z` only when `z` is a standard abbreviation (e.g., `np` for
`numpy`).  
`import y as z` は `z` が標準的な略語のときに使います。（たとえば `numpy` に対する `np`）

For example the module `sound.effects.echo` may be imported as follows:  
たとえば `sound.effects.echo` モジュールは次のようにインポートします：

```python
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```

Do not use relative names in imports. Even if the module is in the same
package, use the full package name. This helps prevent unintentionally
importing a package twice.  
相対名でインポートしないでください。
おなじパッケージ内のモジュールでも完全名をつかいます。
こうすると気づかずおなじパッケージを二度インポートする愚を避けられます。

### 2.3 Packages

パッケージ

Import each module using the full pathname location of the module.  
モジュールの完全パス名でインポートします。

#### 2.3.1 Pros

利点

Avoids conflicts in module names or incorrect imports due to the module search
path not being what the author expected. Makes it easier to find modules.  
モジュール名の衝突を避けたり（モジュール検索パスが作者の意図とちがうために起きる）不正確なインポートを無くせます。
また、こうすることでモジュールを探しやすくなります。

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

No: (assume this file lives in `doctor/who/` where `jodie.py` also exists)  
こうではありません：（`doctor/who/` の下、 `jodie.py` とおなじ場所のファイルだとして）

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

### 2.4 Exceptions

例外

Exceptions are allowed but must be used carefully.  
例外はつかって構いませんが注意してください。


#### 2.4.1 Definition

定義

Exceptions are a means of breaking out of normal control flow to handle errors
or other exceptional conditions.  
例外は通常の制御フローの外に出るために使います。
そしてエラーの処理や例外状況への対処をします。


#### 2.4.2 Pros

利点

The control flow of normal operation code is not cluttered by error-handling
code. It also allows the control flow to skip multiple frames when a certain
condition occurs, e.g., returning from N nested functions in one step instead
of having to plumb error codes through.  
通常処理の制御フローがエラー処理コードでごちゃごちゃしません。
また特定条件下で複数のフレームを飛ばすような制御もできます。
たとえば N 段の関数呼び出しからエラーコードを逐一検査せず１ステップで戻れます。


#### 2.4.3 Cons

欠点

May cause the control flow to be confusing. Easy to miss error cases when
making library calls.  
制御フローがわかりづらくなります。ライブラリー呼び出しでのエラー処理を忘れがちです。


#### 2.4.4 Decision

取り決め

Exceptions must follow certain conditions:  
例外は次の条件にしたがってください：

* Make use of built-in exception classes when it makes sense. For example,
raise a `ValueError` to indicate a programming mistake like a violated
precondition (such as if you were passed a negative number but required a
positive one). Do not use `assert` statements for validating argument values
of a public API. `assert` is used to ensure internal correctness, not to
enforce correct usage nor to indicate that some unexpected event occurred.
If an exception is desired in the latter cases, use a raise statement. For
example:  
組み込みの例外は、その意味が生きるときにつかいます。たとえば `ValueError`
は間違ったコードで事前条件が満たされないときにつかえます（正の値が必要な処理で負の値を受けとったなど）。
公開 API で引数を確認するためには `assert` をつかいません。 `assert` は内部的なただしさを確認するためにつかい、
ただしい利用方法を強制したり不測の事態を知らせるためにつかってはいけません。
不測の事態をつたえるために例外が望ましいならば raise 文を使います。たとえば：
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
      # 書いていない。これは API を誤って使われたときのふるまいを
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
* Libraries or packages may define their own exceptions. When doing so they
must inherit from an existing exception class. Exception names should end in
`Error` and should not introduce stutter (`foo.FooError`).  
ライブラリーやパッケージで独自の例外を定義できます。このときは既存の例外クラスを継承します。
例外名は `Error` で終わるようにします。また `foo.FooError` のような冗長な繰り返しは避けます。
* Never use catch-all `except:` statements, or catch `Exception` or
`StandardError`, unless you are  
すべてを飲み込む `except:` 文や `Exception` や `StandardError` の捕捉は決してつかわないこと。
ただし、
  * re-raising the exception, or  
  その例外を再送出する場合や、
  * creating an isolation point in the program where exceptions are not
  propagated but are recorded and suppressed instead, such as protecting a
  thread from crashing by guarding its outermost block.  
  例外を伝播せず記録して抑止するようなプログラムの隔離場所をつくるとき
  （たとえばスレッドの最外ブロックでクラッシュを防止する）は除きます。  
Python is very tolerant in this regard and `except:` will really catch
everything including misspelled names, sys.exit() calls, Ctrl+C interrupts,
unittest failures and all kinds of other exceptions that you simply don’t
want to catch.  
Python はこのような例外捕捉に従順なうえ `except:` はあらゆるものを捕まえてしまいます。
たとえば名前の間違い、 sys.exit() 呼び出し、 Ctrl+C による割り込み、ユニットテストの失敗、
その他よもや捕まえようとはおもわないあらゆる例外を、です。
* Minimize the amount of code in a `try`/`except` block. The larger the body
of the `try`, the more likely that an exception will be raised by a line of
code that you didn’t expect to raise an exception. In those cases, the
`try`/`except` block hides a real error.  
`try`/`except` ブロックで囲むコードをできるだけ短くしてください。
`try` の本文が大きくなるほど、望まなかったコード行までが例外を投げるようになります。
こうなると `try`/`except` ブロックが本当のエラーを隠してしまいます。
* Use the `finally` clause to execute code whether or not an exception is
raised in the `try` block. This is often useful for cleanup, i.e., closing a
file.  
`finally` 節は `try` ブロック内での例外の発生如何によらず実行したいコードを書くためにつかいます。
`finally` はファイルを閉じるといった後片付けに役立ちます。

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

While they are technically variables, module-level constants are permitted
and encouraged. For example: `_MAX_HOLY_HANDGRENADE_COUNT = 3`. Constants must
be named using all caps with underscores. See Naming below.  
技術的には変数ですが、モジュールレベルの定数は積極的につかいましょう。
たとえば `_MAX_HOLY_HANDGRANADE_COUNT = 3` といったものです。
定数名はすべて大文字とアンダースコア（キャピタルスネークケース）で構成します。
あとの[名づけ](#316-naming)も参照してください。

If needed, globals should be declared at the module level and made internal
to the module by prepending an `_` to the name. External access must be done
through public module-level functions. See Naming below.  
つかう場合は、大域変数はモジュールレベルで宣言し、またモジュール内に限定します。
名前に `_` を前置することでモジュール内部変数とできます。
外部からこれにアクセスするときはモジュールレベルの公開関数をつかいます。
[名づけ](#316-naming)も参照してください。


### 2.6 Nested/Local/Inner Classes and Functions

入れ子の/ローカルの/内部のクラスと関数

Nested local functions or classes are fine when used to close over a local
variable. Inner classes are fine.  
ローカル変数を閉じこむために利用する入れ子のローカル関数やクラスはすばらしい。
内部クラスも最高です。（訳注： closure は関数呼び出し時点の「環境（変数とその値のペア）」
を閉じこむテクニック。時間のかかる計算を閉じこんでおき、あとで実行するなど）


#### 2.6.1 Definition

定義

A class can be defined inside of a method, function, or class. A function can
be defined inside a method or function. Nested functions have read-only access
to variables defined in enclosing scopes.  
クラスはメソッドや関数、クラスの中で定義できます。関数もメソッドや関数の中で定義できます。
入れ子の関数は、その関数の外のスコープの変数を読むこと（だけ）ができます。

#### 2.6.2 Pros

利点

Allows definition of utility classes and functions that are only used inside
of a very limited scope. Very
[ADT](http://www.google.com/url?sa=D&q=http://en.wikipedia.org/wiki/Abstract_data_type)-y.
Commonly used for implementing decorators.  
限定されたスコープの中でだけつかえる補助クラスや関数を定義できます。非常に
[ADT](http://www.google.com/url?sa=D&q=http://en.wikipedia.org/wiki/Abstract_data_type)
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
just to hide it from users of a module. Instead, prefix its name with an `_`
at the module level so that it can still be accessed by tests.  
いくつか注意は必要ですがすばらしいものです。
（`self` と `cls` を除く）ローカル変数を閉じこむ以外の目的で関数やクラスを入れ子にしてはいけません。
関数をモジュールの利用者から隠すために入れ子にしてはいけません。
この目的ならば `_` を前置してモジュールレベルで定義します（これならテストもできます）。


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

Simple comprehensions can be clearer and simpler than other dict, list, or
set creation techniques. Generator expressions can be very efficient, since
they avoid the creation of a list entirely.  
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
複数の `for` 節や filter 式を使ってはいけません。複雑になるようならループを使います。

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


### 2.8 Default Iterators and Operators

デフォルトイテレーターと演算子

Use default iterators and operators for types that support them, like lists,
dictionaries, and files.  
リストや辞書、ファイルなど、型に用意されている既定のイテレーターや演算子を使いましょう。


#### 2.8.1 Definition

定義

Container types, like dictionaries and lists, define default iterators and
membership test operators (“in” and “not in”).  
コンテナ型（辞書やリストなど）は既定のイテレーターや包含確認演算（“in” や
“not in”）を定義しています。

#### 2.8.2 Pros

利点

The default iterators and operators are simple and efficient. They express
the operation directly, without extra method calls. A function that uses
default operators is generic. It can be used with any type that supports the
operation.  
規定のイテレーターや演算子は単純で効率的です。
これらは操作そのものを表現しており、あらためてメソッド呼び出す必要はありません。
既定の演算子で書いた関数は汎用的です。その演算子をもつ型すべてにつかえるのです。

#### 2.8.3 Cons

欠点

You can’t tell the type of objects by reading the method names (e.g.
`has_key()` means a dictionary). This is also an advantage.  
メソッド名からだけではオブジェクトの型がわかりません（`has_key()`
は辞書を暗示するだけです）。これは利点でもあります。

#### 2.8.4 Decision

取り決め

Use default iterators and operators for types that support them, like lists,
dictionaries, and files. The built-in types define iterator methods, too.
Prefer these methods to methods that return lists, except that you should
not mutate a container while iterating over it.  
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


### 2.9 Generators

ジェネレーター

Use generators as needed.  
必要に応じてつかってください。

#### 2.9.1 Definition

定義

A generator function returns an iterator that yields a value each time it
executes a `yield` statement. After it yields a value, the runtime state of
the generator function is suspended until the next value is needed.  
ジェネレーター関数は `yield` 文を実行するたびに値を生成するイテレーターを返します。
関数は値を生成すると、次の値が要求されるまでそこで一旦停止状態になります。

#### 2.9.2 Pros

利点

Simpler code, because the state of local variables and control flow are
preserved for each call. A generator uses less memory than a function that
creates an entire list of values at once.  
呼び出しのたびにローカル変数と実行位置（訳注：原文は controll flow）の状態が保存されるため、
コードが簡単になります。リスト全体を生成する関数とくらべて、ジェネレーターはメモリーを消費しません。

#### 2.9.3 Cons

欠点

None.  
なし。

#### 2.9.4 Decision

取り決め

Fine. Use “Yields:” rather than “Returns:” in the docstring for generator
functions.  
すばらしい。
ジェネレーター関数の docstring では “Returns:” でなく “Yields:” をつかいます。


### 2.10 Lambda Functions

無名関数

Okay for one-liners. Prefer generator expressions over `map()` or `filter()`
with a `lambda`.  
一行に収まるならつかってよいです。
無名関数を `map()` や `filter()` とともにつかうよりジェネレーター式を使ってください。

#### 2.10.1 Definition

定義

Lambdas define anonymous functions in an expression, as opposed to a
statement.  
ラムダは文でなく式として無名関数を定義します。

#### 2.10.2 Pros

利点

Convenient.  
便利です。


#### 2.10.3 Cons

欠点

Harder to read and debug than local functions. The lack of names means stack
traces are more difficult to understand. Expressiveness is limited because
the function may only contain an expression.  
ローカル関数定義にくらべ読みづらく、デバッグしづらくなります。（関数に）名前がつかないため、
スタックトレースを理解しづらくなります。
（無名関数内に）ただひとつの式しか書けないため表現できるコードに限界があります。

#### 2.10.4 Decision

取り決め

Okay to use them for one-liners. If the code inside the lambda function is
longer than 60-80 chars, it’s probably better to define it as a regular
[nested function](#216-lexical-scoping).  
一行に収まるならつかってよいです。無名関数内が 60 ～ 80 文字をこえるならば、
おそらく[入れ子の関数](#216-lexical-scoping)をつかうとよいでしょう。

For common operations like multiplication, use the functions from the
`operator` module instead of lambda functions. For example, prefer
`operator.mul` to `lambda x, y: x * y`.  
乗算のような一般的な演算は `operator` モジュール定義の関数をつかってください。
つまり `lambda x, y: x * y` と書くかわりに `operator.mul` を使います。
（訳注：たとえば `product` 関数を `product = lambda vs: functools.reduce(operator.mul, vs)` のように定義する）


### 2.11 Conditional Expressions

条件式

Okay for simple cases.  
簡単なものならつかってよいです。


#### 2.11.1 Definition

定義

Conditional expressions (sometimes called a “ternary operator”) are mechanisms
that provide a shorter syntax for if statements. For example:
`x = 1 if cond else 2`.  
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
複雑になるなら if 文を使います。（訳注：先の `x = 1 if cond else 2` で言えば `1` が真の式、
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

### 2.12 Default Argument Values

デフォルト引数

Okay in most cases.  
おおむね、つかってよいです。

#### 2.12.1 Definition

定義

You can specify values for variables at the end of a function’s parameter list,
e.g., `def foo(a, b=0):`. If `foo` is called with only one argument, `b` is set
to 0. If it is called with two arguments, `b` has the value of the second
argument.  
たとえば `def foo(a, b=0):` のように関数の引数には値を指定できます（訳注：末尾から順に、間をあけずに）。
`foo` を引数ひとつで呼びだすと `b` は 0 になります。二引数で呼びだせば `b` はふたつめの値になります。

#### 2.12.2 Pros

利点

Often you have a function that uses lots of default values, but on rare
occasions you want to override the defaults. Default argument values provide
an easy way to do this, without having to define lots of functions for the rare
exceptions. As Python does not support overloaded methods/functions, default
arguments are an easy way of “faking” the overloading behavior.  
たくさんの規定値をほとんど変えずに使える関数をつくるとします。
デフォルト引数はこれを実現する簡単な方法です。
すなわち、まれな例外ケースのためにたくさんの関数をつくらずに済みます。
（訳注：引数ひとつ版、一個飛ばし版などのオーバーロードをたくさん定義せずにすむ）  
Python では関数やメソッドをオーバーロードできませんが、
デフォルト引数でオーバーロード的なふるまいを実現できます。

#### 2.12.3 Cons

欠点

Default arguments are evaluated once at module load time. This may cause
problems if the argument is a mutable object such as a list or a dictionary.
If the function modifies the object (e.g., by appending an item to a list),
the default value is modified.  
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


### 2.13 Properties

プロパティ

Use properties for accessing or setting data where you would normally
have used simple, lightweight accessor or setter methods.  
簡単で軽量な getter/setter をつかってきた場所では、データへのアクセス・設定にプロパティをつかいます。

#### 2.13.1 Definition

定義

A way to wrap method calls for getting and setting an attribute as a
standard attribute access when the computation is lightweight.  
（計算時間が軽い）メソッド呼び出しで属性の値の読み出しと設定をするところで、
これを通常の属性へのアクセスのようにラップする方法です。

#### 2.13.2 Pros

利点

Readability is increased by eliminating explicit get and set method calls
for simple attribute access. Allows calculations to be lazy. Considered the
Pythonic way to maintain the interface of a class. In terms of performance,
allowing properties bypasses needing trivial accessor methods when a direct
variable access is reasonable. This also allows accessor methods to be added
in the future without breaking the interface.  
簡単な属性アクセスの getter/setter メソッド呼び出しをなくせるため可読性が高まります。
計算を遅らせられます。クラスのインターフェースを維持する Python らしさが考慮されています。
性能面では、変数への直接アクセスがよさそうなら、些末なアクセッサーメソッドをなくすように実装することもできます。
将来、インターフェースを壊さずアクセッサーメソッドを追加することもできます。

#### 2.13.3 Cons

欠点

Can hide side-effects much like operator overloading. Can be confusing
for subclasses.  
演算子のオーバーロードのように副作用が見えづらくなります。
サブクラスの実装で混乱するかもしれません。

#### 2.13.4 Decision

取り決め

Use properties in new code to access or set data where you would normally
have used lightweight accessor or setter methods. Properties should be created
with the `@property` [decorator](#217-function-and-method-decorators).  
あたらしく書くコードではデータの読みとり、設定をするときにプロパティをつかいましょう
（これまでに軽量の getter/setter を書いていたところ）。プロパティは
`@property` [デコレーター](#217-function-and-method-decorators)で作成します。

Inheritance with properties can be non-obvious if the property itself is
not overridden. Thus one must make sure that accessor methods are called
indirectly to ensure methods overridden in subclasses are called by the
property (using the template method design pattern).  
プロパティをつかった継承はプロパティそのものをオーバーライドしていないとわかりづらいかもしれません。
プロパティが、オーバーライドしたアクセッサーメソッドを間接的に呼びだしていないことを確かめなければならないからです
（テンプレートメソッドのデザインパターンをつかっているときなど）。

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


### 2.14 True/False Evaluations

真と偽の評価

Use the “implicit” false if at all possible.  
可能なところではすべて「暗黙の」 false を使います。

#### 2.14.1 Definition

定義

Python evaluates certain values as `False` when in a boolean context. A quick
“rule of thumb” is that all “empty” values are considered false, so `0`,
`None`, `[]`, `{}`, `''` all evaluate as false in a boolean context.  
Python ではいくつかの値がブール値の文脈で `False` になります。
大雑把にいうと「空」の値は偽になります。たとえば `0`、 `None`、 `[]`、 `{}` や `’’`
はブール値として評価すると偽になります。

#### 2.14.2 Pros

利点

Conditions using Python booleans are easier to read and less error-prone.
In most cases, they’re also faster.  
Python のブール値による条件式は読みやすく、また間違いを起こしにくくなっています。
ほとんどの場合、高速でもあります。

#### 2.14.3 Cons

欠点

May look strange to C/C++ developers.  
C/C++ 開発者には不思議におもえるかもしれません。

#### 2.14.4 Decision

取り決め

Use the “implicit” false if possible, e.g., `if foo:` rather than
`if foo != []:`. There are a few caveats that you should keep in mind though:  
可能なところではすべて「暗黙の」 false を使います。たとえば `if foo != []:` でなく
`if foo:` と書きます。ただしいくつか注意点がありますので覚えておいてください：

* Always use `if foo is None:` (or `is not None`) to check for a `None` value.
E.g., when testing whether a variable or argument that defaults to None was
set to some other value. The other value might be a value that’s false in a
boolean context!  
None はかならず `if foo is None` （あるいは `is not None`）で確認してください。
変数や引数のデフォルト値 `None` が別の値かを確かめるときなどです。
ほかにもブール値として偽と評価される値があります！
* Never compare a boolean variable to `False` using `==`. Use `if not x:`
instead. If you need to distinguish `False` from `None` then chain the
expressions, such as `if not x and x is not None:`.  
ブール変数を `False` と `==` で比較せず、代わりに `if not x:` とします。
`False` と `None` を判別するには `if not x and x is not None:`
のように式をつなぎます。
* For sequences (strings, lists, tuples), use the fact that empty sequences
are false, so `if seq:` and `if not seq:` are preferable to `if len(seq):`
and `if not len(seq):` respectively.  
並び（文字列、リスト、タプル）では、空の並びが偽であることを利用して `if seq:`
あるいは `if not seq:` のように書きます。（`if len(seq):` や `if not len(seq):`
より望ましい書きかたです）
* When handling integers, implicit false may involve more risk than benefit
(i.e., accidentally handling `None` as 0). You may compare a value which
is known to be an integer (and is not the result of `len()`) against the
integer 0.  
整数の処理では暗黙の偽への変換は利点にくらべ危険が大きくなります
（つまり `None` が誤って 0 になる、など）。整数になることが確実な値は 0
と比較したほうがよいでしょう。（ただし `len()` の結果は除く）

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
* Note that `'0'` (i.e., `0` as string) evaluates to true.  
（数値の `0` でなく文字列の） `'0'` は真になることに注意。


### 2.16 Lexical Scoping

文脈スコープ

Okay to use.  
つかってヨシ！


#### 2.16.1 Definition

定義

A nested Python function can refer to variables defined in enclosing functions,
but cannot assign to them. Variable bindings are resolved using lexical
scoping, that is, based on the static program text. Any assignment to a name
in a block will cause Python to treat all references to that name as a local
variable, even if the use precedes the assignment. If a global declaration
occurs, the name is treated as a global variable.  
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
experienced Lisp and Scheme (and Haskell and ML and …) programmers.  
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

So `foo([1, 2, 3])` will print `1 2 3 3`, not `1 2 3 4`.
よって `foo([1, 2, 3])` の呼びだしは `1 2 3 3` を表示します。（`1 2 3 4` にはなりません）

#### 2.16.4 Decision

取り決め

Okay to use.  
おつかいください。


### 2.17 Function and Method Decorators

関数とメソッドのデコレーター

Use decorators judiciously when there is a clear advantage.
Avoid `staticmethod` and limit use of `classmethod`.  
明確な利点があると判断できるときにデコレーターをつかいます。
`staticmethod` はつかわないでください。 `classmethod` の利用は控えてください。

#### 2.17.1 Definition

定義

[Decorators for Functions and Methods](https://docs.python.org/3/glossary.html#term-decorator)
(a.k.a “the `@` notation”). One common decorator is `@property`, used for
converting ordinary methods into dynamically computed attributes. However,
the decorator syntax allows for user-defined decorators as well. Specifically,
for some function `my_decorator`, this:  
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

Decorators can perform arbitrary operations on a function’s arguments or
return values, resulting in surprising implicit behavior. Additionally,
decorators execute at import time. Failures in decorator code are pretty
much impossible to recover from.  
デコレーターは関数の引数や戻り値にたいしてあらゆる操作ができるため、
驚くような予期せぬ結果が生じます。加えてデコレーターはインポート時に処理されます。
デコレーターの実装に問題があると、ここから復帰することはほぼ不可能です。

#### 2.17.4 Decision

取り決め

Use decorators judiciously when there is a clear advantage. Decorators
should follow the same import and naming guidelines as functions. Decorator
pydoc should clearly state that the function is a decorator. Write unit
tests for decorators.  
明確な利点があると判断できるときだけデコレーターをつかいます。
デコレーターは関数と同じインポートと名前づけのガイドラインにしたがってください。
デコレーターの docstring では、それがデコレーターであると明記してください。
デコレーター（の定義）にはユニットテストを書きます。

Avoid external dependencies in the decorator itself (e.g. don’t rely on
files, sockets, database connections, etc.), since they might not be
available when the decorator runs (at import time, perhaps from `pydoc`
or other tools). A decorator that is called with valid parameters should
(as much as possible) be guaranteed to succeed in all cases.  
デコレーターに外部への依存を持ちこまないでください（つまり、ファイルやソケット、
データベース接続などに頼らない）。これらはデコレーターの解釈時に利用できないかもしれません
（解釈はインポート時におこなわれ、 pydoc などのツールもインポートはします）。
ただしく呼びだされたデコレーターはいかなるときも必ず成功するようにしてください。

Decorators are a special case of “top level code” - see [main](#317-main)
for more discussion.  
デコレーターは「トップレベルコード」の特殊な場合です。詳細は [main](#317-main)
の節を参照してください。

Never use `staticmethod` unless forced to in order to integrate with an
API defined in an existing library. Write a module level function instead.  
`staticmethod` の利用は禁止です。
（既存のライブラリーに定義された API と結合するために指定されている場合は除きます）  
代わりにモジュールレベルの関数を定義してください。

Use `classmethod` only when writing a named constructor or a class-specific
routine that modifies necessary global state such as a process-wide cache.  
`classmethod` は名前つきコンストラクターや、
プロセスワイドなキャッシュのような大域的状態の変更が避けられないクラス固有の処理を定義するときに（のみ）つかいます。


### 2.18 Threading

マルチスレッド

Do not rely on the atomicity of built-in types.  
組み込み型の不可分性にたよらないでください。

While Python’s built-in data types such as dictionaries appear to have
atomic operations, there are corner cases where they aren’t atomic (e.g. if
`__hash__` or `__eq__` are implemented as Python methods) and their atomicity
should not be relied upon. Neither should you rely on atomic variable
assignment (since this in turn depends on dictionaries).  
辞書のような Python 組み込みデータ型が不可分操作を提供しているように見えても、
これらには不可分ではないエッジケースがあります（`__hash__` や `__eq__` が Python
のメソッドとして実装されている場合など）。そしてその不可分性をあてにしてはならないのです。
もちろん代入の不可分性もあてにしてはいけません（それが辞書になっていたとしてもです）。

Use the Queue module’s `Queue` data type as the preferred way to communicate
data between threads. Otherwise, use the threading module and its locking
primitives. Prefer condition variables and `threading.Condition` instead of
using lower-level locks.  
スレッド間でのデータの受けわたしには Queue モジュールの `Queue` データ型をつかいましょう。
そうでなければ threading モジュールと、これのロック用プリミティブをつかいます。
その他の低レベルロックでなく条件変数 `threading.Condition` をつかってください。
