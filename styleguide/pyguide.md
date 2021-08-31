# Google Python Style Guide

Table of Contents
* [1 Background](#1-background)
* [2 Python Language Rules](#2-python-language-rules)
  * [2.1 Lint](#21-lint)

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

`pylint` is a tool for finding bugs and style problems in Python source code. It finds problems that are typically caught by a compiler for less dynamic languages like C and C++. Because of the dynamic nature of Python, some warnings may be incorrect; however, spurious warnings should be fairly infrequent.  
`pylint` は Python ソースコードにひそむバグや書式の問題をみつけるツールです。これは C や C++ のような動的性の低い言語でコンパイラーがつかまえる類の問題をみつけてくれます。 Python の動的な性質のためにときに警告が不正確になりますが、間違うことはほとんどありません。

#### 2.1.2 Pros

利点

Catches easy-to-miss errors like typos, using-vars-before-assignment, etc.  
ミスタイプや定義前の変数参照といった見落としがちな間違いをみつけられます。

#### 2.1.3 Cons

欠点

`pylint` isn’t perfect. To take advantage of it, sometimes we’ll need to write around it, suppress its warnings or fix it.  
`pylint` は完璧ではありません。結果を得るためには書き加えたり抑止したり修正したりといった作業が必要です。

#### 2.1.4 Decision

取り決め

Make sure you run pylint on your code.  
かならず `pylint` をコードに適用してください。

Suppress warnings if they are inappropriate so that other issues are not hidden. To suppress warnings, you can set a line-level comment:  
（そのためにほかの問題が消えなくなるような）不適切な警告は抑止します。抑止は行単位のコメントで設定します：

```python
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
```

`pylint` warnings are each identified by symbolic name (`empty-docstring`) Google-specific warnings start with `g-`.  
`pylint` 警告には固有のシンボル名があります（`empty-docstring` など）。 Google が定義した警告名は `g-` ではじまります。

If the reason for the suppression is not clear from the symbolic name, add an explanation.  
抑止した理由がシンボル名から判然としないなら説明をくわえてください。

Suppressing in this way has the advantage that we can easily search for suppressions and revisit them.  
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

Unused argument warnings can be suppressed by deleting the variables at the beginning of the function. Always include a comment explaining why you are deleting it. “Unused.” is sufficient. For example:  
未使用の引数の警告には関数冒頭で削除して対応してください。削除理由もあわせてコメントしてください。「未使用」で充分です。たとえば：

```python
def viking_cafe_order(spam: str, beans: str, eggs: Optional[str] = None) -> str:
    del beans, eggs  # Unused by vikings.
    return spam + spam + spam
```

Other common forms of suppressing this warning include using ‘`_`’ as the identifier for the unused argument or prefixing the argument name with ‘`unused_`’, or assigning them to ‘`_`’. These forms are allowed but no longer encouraged. These break callers that pass arguments by name and do not enforce that the arguments are actually unused.  
警告に対応するその他の方法もあります。変数名を ‘`_`’ にしたり引数名に ‘`unused_`’ を前置したり ‘`_`’ に代入するなどです。これら方法をつかってもかまいませんがお勧めしません。これらの方法は引数名を指定した呼びだしを壊しますし、引数がつかわれないことも保証できません。
