# Google Python Style Guide

Table of Contents
* [1 Background](#1-background)
* [2 Python Language Rules](#2-python-language-rules)
  * [2.1 Lint](#21-lint)
  * [2.2 Imports](#22-imports)
  * [2.3 Packages](#23-packages)

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
