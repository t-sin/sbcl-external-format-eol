# SBCL改行コード対応めも

SBCLが現状扱える改行コードは実行プラットフォームもののみ(Linuxなら`LF`)だが、これを`LF`/`CR`/`CRLF`全て扱えるようにしたい。

# 現状認識

- SBCLはexternal-formatで改行コードを扱えない
    - LinuxでCRLFなファイルを開くと`#\Return`がいっぱい現れる
- SBCLはプラットフォーム標準の改行コードを`#\Newline`として扱っている

# わかったこと

- SBCLのビルドには別のCommon Lisp処理系が必要となる(!)
    - ホストのCommon Lisp処理系との境界部分で改行コードを上手く処理できればいい

# 目標

- external-formatに改行コードが増える
- 入出力においてプラットフォームに関係なく`CR`/`LF`/`CRLF`扱えるようにする
- テストされている

# TODO

- [ ] ファイル洗い出し
- [ ] 調査する
- [ ] テスト考える
- [ ] 実装する
- [ ] パッチつくる
- [ ] パッチ出す

## 要対応かもしれないファイル

バージョン

```
sbcl $ git log --oneline -n 2
2de7232 Transform calls to make-array with fill-pointer or adjustable.
764b322 Remove conditionalization for sxhash of conditions.
```

調べるのに使ったコマンド

```sh
$ git grep -i '#\\newline' | cut -d ':' -f 1 | sort | uniq | sed -Ee 's/^/- [ ] /'
```

おそらく、高レベルな部分(`dsrc/code/describe.lisp`とか上のレベルっぽい)は弄らなくていいはず。低レベルな部分はexternal-formatに関わるので、調査が必要。


- [-] ~~NEWS~~
- [x] contrib/sb-aclrepl/debug.lisp
    - AllegroCL的なREPLを実現
    - 改行を出力(with `princ`)。不要。
- [x] contrib/sb-aclrepl/inspect.lisp
    - AllegroCL的なREPLを実現
    - 改行を出力(with `princ`)。不要。
- [ ] contrib/sb-aclrepl/repl.lisp
- [ ] contrib/sb-cover/cover.lisp
- [ ] contrib/sb-simple-streams/impl.lisp
- [ ] contrib/sb-simple-streams/internal.lisp
- [ ] contrib/sb-simple-streams/strategy.lisp
- [ ] contrib/sb-simple-streams/terminal.lisp
- [-] ~~doc/GIT-FOR-SBCL-HACKERS.txt~~
- [-] ~~doc/manual/gray-streams-examples.texinfo~~

    - 内部でさらにストリームを扱ってるので、external-formatの抽象化の上っぽい

- [ ] src/code/cross-char.lisp
- [ ] src/code/deftypes-for-target.lisp
- [ ] src/code/describe.lisp
- [ ] src/code/early-format.lisp
- [ ] src/code/fd-stream.lisp
- [ ] src/code/hpux-os.lisp
- [ ] src/code/late-extensions.lisp
- [ ] src/code/late-format.lisp
- [ ] src/code/linux-os.lisp
- [ ] src/code/load.lisp
- [ ] src/code/osf1-os.lisp
- [ ] src/code/pprint.lisp
- [ ] src/code/reader.lisp
- [ ] src/code/run-program.lisp
- [ ] src/code/stream.lisp
- [ ] src/code/sunos-os.lisp
- [ ] src/code/target-format.lisp
- [ ] src/compiler/srctran.lisp
- [ ] src/pcl/gray-streams.lisp
- [ ] tests/assembler.pure.lisp
- [ ] tests/case.pure.lisp
- [ ] tests/compiler.impure.lisp
- [ ] tests/compiler.pure.lisp
- [ ] tests/debug.impure.lisp
- [ ] tests/external-format.impure.lisp
- [ ] tests/filesys.pure.lisp
- [ ] tests/gray-streams.impure.lisp
- [ ] tests/interface.impure.lisp
- [ ] tests/interface.pure.lisp
- [ ] tests/pprint.impure.lisp
- [ ] tests/print.impure.lisp
- [ ] tests/reader.pure.lisp
- [ ] tests/run-program.impure.lisp
- [ ] tests/run-program.test.sh
- [ ] tests/stream.impure.lisp
- [ ] tests/stream.test.sh
- [ ] tests/walk.impure.lisp
