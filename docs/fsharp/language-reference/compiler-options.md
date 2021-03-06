---
title: コンパイラ オプション (F#)
description: F# のコンパイラ コマンド ライン オプションを使用すると、f# アプリとライブラリのコンパイルを制御できます。
ms.date: 05/16/2016
ms.openlocfilehash: f4a596d0dede6f66faa34374f7f73a89323f1620
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33566445"
---
# <a name="compiler-options"></a>コンパイラ オプション

このトピックでは、F# コンパイラ (fsc.exe) のコマンド ライン オプションについて説明します。 コンパイル環境は、プロジェクトのプロパティを設定して制御することもできます。


## <a name="compiler-options-listed-alphabetically"></a>アルファベット順のコンパイラ オプション
次の表は、コンパイラ オプションのアルファベット順の一覧です。 一部の F# コンパイラ オプションは C# コンパイラ オプションに似ています。 それらについては、C# コンパイラ オプションのトピックへのリンクも用意されています。



|コンパイラ オプション|説明|
|---------------|-----------|
|**-a****&lt;output-filename&gt;**|ライブラリを生成し、そのファイル名を指定します。 このオプションの短い形式は、 **--ターゲット:ライブラリ****&lt;filename&gt;** です。|
|**--baseaddress:&lt;string&gt;**|ビルドするライブラリのベース アドレスを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;baseaddress &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/2fdbz5xd.aspx)です。|
|**--codepage:&lt;int&gt;**|ソース ファイルの読み取りに使用するコードページを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;コードページ&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/w0kyekyh.aspx)です。|
|**--consolecolors**|エラーおよび警告がコンソールの色分けされたテキストを使用することを指定します。|
|**--crossoptimize**[**+**&#124;**-**]|モジュール間の最適化を有効または無効にします。|
|**--delaysign**[**+**&#124;**-**]|厳密な名前のキーのパブリックな部分のみを使ってアセンブリに遅延署名します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;delaysign &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/ta1sxwy8.aspx)です。|
|**--checked**[**+**&#124;**-**]|オーバーフロー チェックの生成を有効または無効にします。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;チェック&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/h25wtyxf.aspx)です。|
|**--debug**[**+**&#124;**-**]<br /><br />**-g**[**+**&#124;**-**]<br /><br />**--debug**:[**full**&#124;**pdbonly**]<br /><br />**-g:** [**full**&#124;**pdbonly**]|デバッグ情報の生成を有効または無効にしたり、生成するデバッグ情報の種類を指定したりします。 既定値は、実行中のプログラムにアタッチできる full です。 選択**pdbonly** pdb (プログラム データベース) ファイルに格納されている制限のデバッグ情報を取得します。<br /><br />同じ名前の C# コンパイラ オプションに相当します。 詳細については、次のトピックを参照してください。<br /><br />[&#47;デバッグ&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/8cw0bt21.aspx)です。|
|**--define:&lt;string&gt;**<br /><br />**-d:&lt;string&gt;**|条件付きコンパイルで使用する記号を定義します。|
|**--deterministic**[**+**&#124;**-**]|(モジュールのバージョン GUID とタイムスタンプを含む)、決定論的のアセンブリを生成します。  これは、バージョン番号のワイルドカードでは使用できず、のみ埋め込み、移動可能な種類のデバッグをサポート|
|**--doc:&lt;xmldoc-filename&gt;**|指定したファイルに XML ドキュメント コメントを生成するようにコンパイラに指示します。 詳細については、次を参照してください。 [XML ドキュメント](xml-documentation.md)です。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;doc &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/3260k4x7.aspx)です。|
|**--fullpaths**|絶対パスを生成するようコンパイラに指示します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;fullpaths &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/d315xc66.aspx)です。|
|**--help**<br /><br />**-?**|使用方法を表示します。すべてのコンパイラ オプションの簡単な説明が表示されます。|
|**--highentropyva**[**+**&#124;**-**]|強化されたセキュリティ機能である、高いエントロピの ASLR (Address Space Layout Randomization) を有効または無効にします。 OS は、アプリケーションのインフラストラクチャ (スタックとヒープなど) が読み込まれるメモリの位置をランダム化します。 このオプションを有効にすると、オペレーティング システムではこのランダム化を使用して、64 ビット コンピューターで完全な 64 ビット アドレス空間を使用できます。|
|**--keycontainer:&lt;文字列&gt;**|厳密な名前のキー コンテナーを指定します。|
|**--keyfile:&lt;ファイル名&gt;**|生成されるアセンブリの署名に使用する公開キー ファイルの名前を指定します。|
|**--lib:&lt;folder-name&gt;**<br /><br />**-I:&lt;folder-name&gt;**|参照されるアセンブリを検索するディレクトリを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;lib &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/s5bac5fx.aspx)です。|
|**--linkresource**:**&lt;resource-info&gt;**|指定されたリソースをアセンブリにリンクさせます。 リソース情報の形式が*filename*[、*名前*[、*パブリック*&#124;*プライベート*]<br /><br />代わりに、埋め込みリソース ファイル全体では、このオプションを 1 つのリソースをリンク、 **--リソース**オプション。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;linkresource &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/xawyf94k.aspx)です。|
|**mlcompatibility--**|他のバージョンの ML との互換性維持を目的に設計された機能を使用するときに、表示される警告を無視します。|
|**--noframework**|.NET Framework アセンブリへの既定の参照を無効にします。|
|**--nointerfacedata**|F# 固有のメタデータを含むアセンブリに通常は追加されるリソースを省略するよう、コンパイラに指示します。|
|**--nologo**|コンパイラの起動時にバナー テキストが表示されないようにします。|
|**--nooptimizationdata**|インライン構成要素の実装に必要な場合にのみ最適化を行うよう、コンパイラに指示します。 モジュール間のインライン処理を禁止し、バイナリの互換性を改善してください。|
|**--nowin32manifest**|既定の Win32 マニフェストを省略するようコンパイラに指示します。|
|**--nowarn:&lt;int-list&gt;**|番号で指定した特定の警告を無効にします。 それぞれの警告番号はコンマで区切ります。 警告の警告番号はコンパイルの出力で確認できます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;nowarn &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/7f28x9z3.aspx)です。|
|**--optimize**[**+**&#124;**-**]**[&lt;string-list&gt;]**<br /><br />**-O[+&#124;-] [&lt;string-list&gt;]**|最適化を有効または無効にします。 一部の最適化オプションについては、選択して指定したものだけを無効または有効にすることができます。 これらは、: **nojitoptimize**、 **nojittracking**、 **nolocaloptimize**、 **nocrossoptimize**、 **notailcalls**.|
|**--out:&lt;output-filename&gt;**<br /><br />**-o:&lt;output-filename&gt;**|コンパイルしたアセンブリまたはモジュールの名前を指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;アウト&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/bw3t50f3.aspx)です。|
|**--pdb:&lt;pdb-filename&gt;**|出力デバッグ PDB (プログラム データベース) ファイルに名前を付けます。 このオプション場合のみ適用されます **--デバッグ**も有効にします。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;pdb &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/ms228625.aspx)です。|
|**--platform:&lt;platform-name&gt;**|生成されたコードは、指定したプラットフォームでのみ実行されるを指定します (**x86**、 **Itanium**、または**x64**)、またはの場合、プラットフォーム名**anycpu**選択した場合、生成されたコードが任意のプラットフォームで実行されていることを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;プラットフォーム&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/zekwfyz4.aspx)です。|
|**--preferreduilang:&lt;lang&gt;**| (たとえば、ES-ES, JA-JP) 出力用の言語のカルチャ名を指定します。 |
|**--quotations-debug**|追加のデバッグ情報が F# 引用符リテラルとリフレクション定義から派生した式に対して生成されるように指定します。 デバッグ情報は F# 式ツリー ノードのカスタム属性に追加されます。 参照してください[コード クォート](code-quotations.md)と[Expr.CustomAttributes](https://msdn.microsoft.com/library/eb89943f-5f5b-474e-b125-030ca412edb3)です。|
|**--reference:&lt;assembly-filename&gt;**<br /><br />**-r** &lt;**assembly-filename&gt;**|コンパイルするコードで F# または .NET Framework アセンブリのコードを使用できるようにします。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;参照&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/yabyz3h4.aspx)です。|
|**--resource:&lt;resource-filename&gt;**|生成されるアセンブリにマネージ リソース ファイルを埋め込みます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;リソース&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/c0tyye07.aspx)です。|
|**--sig**:&lt;**signature-filename&gt;**|生成されるアセンブリに基づいてシグネチャ ファイルを生成します。 シグネチャ ファイルの詳細については、次を参照してください。[署名](signatures.md)です。|
|**--simpleresolution**|MSBuild の解決方法ではなくディレクトリベースの Mono 規則を使用して解決する必要がある、アセンブリ参照を指定します。 既定では、Mono で実行する場合を除き、MSBuild の解決方法が使用されます。|
|**--standalone**|F# ライブラリなどの追加のアセンブリを行わなくても、単独で実行できるように、すべての依存関係を含むアセンブリを生成するように指定します。|
|**--staticlink:&lt;assembly-name**&gt;|指定したアセンブリと、そのアセンブリに依存するすべての参照 DLL を静的にリンクします。 DLL 名ではなく、アセンブリ名を使用します。|
|**--subsystemversion**|生成された実行可能ファイルが使用できる OS サブシステムのバージョンを指定します。 Windows 8.1、Windows 7、Windows Vista 用 6.00 の場合は 6.01 6.02 を使用します。 このオプションは、DLL ではなく、実行可能ファイルのみに適用され、アプリケーションが OS の特定のバージョンでのみ使用できる特定のセキュリティ機能に依存している場合にのみ使用される必要があります。 このオプションが使用され、低いバージョンの OS でアプリケーションを実行しようとすると、失敗してエラー メッセージが表示されます。|
|**--tailcalls**[**+**&#124;**-**]|tail IL 命令の使用を有効または無効にします。有効にすると、スタック フレームが tail 再帰関数で再利用されます。 既定では、このオプションは有効になっています。|
|**--target**:[**exe** &#124; **winexe** &#124; **library** &#124; **module** ] **&lt;output-filename&gt;**|生成されるコンパイル済みコードの種類とファイル名を指定します。<ul><li>**exe**コンソール アプリケーションのことを意味します。<br /></li><li>**winexe** Windows アプリケーション、コンソール アプリケーションの違いいる標準入出力ストリーム (stdin、stdout、stderr) 定義があるないことを意味します。<br /></li><li>**ライブラリ**エントリ ポイントのないアセンブリです。<br /></li><li>**モジュール**後で組み合わせて使用できるその他のモジュールをアセンブリに .NET Framework モジュール (.netmodule) です。<br /></li><ul/>このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;ターゲット&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/6h25dztx.aspx)です。|
|**--times**|コンパイルのタイミング情報を表示します。|
|**--utf8output**|UTF-8 エンコーディングでのコンパイラ出力を有効にします。|
|**-警告:&lt;警告レベル&gt;**|警告レベル (0 ～ 5) を設定します。 既定のレベルは 3 です。 各警告には、重大度に基づいてレベルが設定されます。 レベル 5 の方が重大度は低くなりますが、レベル 1 より多くの警告が表示されます。<br /><br />レベル 5 の警告は、: 21 (実行時にチェック再帰を使用)、22 (**let rec**順序評価)、45 (完全な抽象化)、および 52 (防御的コピー) します。 他の警告はすべてレベル 2 です。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [&#47;警告&#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/13b90fz7.aspx)です。|
|**--warnon:&lt;int-list&gt;**|別のコマンド ライン オプションによって既定または無効になる可能性がある特定の警告を有効にします。 F# 3.0 では、1182 (使用されていない変数) の警告は既定ではオフに設定されています。|
|**--warnaserror**[**+**&#124;**-**] [**&lt;int-list&gt;**]|警告をエラーとして報告するオプションを有効または無効にします。 特定の警告番号を指定して無効または有効にすることができます。 後のコマンド ラインのオプションが前のコマンド ラインのオプションをオーバーライドします。 たとえば、エラーとして報告される必要のない警告を指定するには、次のように指定します。 **--warnaserror +--warnaserror-:&lt;int リスト&gt;** です。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;warnaserror &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/406xhdz3.aspx)です。|
|**--win32manifest:manifest-filename**|コンパイルに Win32 マニフェスト ファイルを追加します。 このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;win32manifest &#40;C&#35;コンパイラ オプション&#41;](https://msdn.microsoft.com/library/bb545961.aspx)です。|
|**--win32res:resource-filename**|コンパイルに Win32 リソース ファイルを追加します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、次を参照してください。 [ &#47;win32res (&#40;C & #35)。コンパイラ オプション&#41;](https://msdn.microsoft.com/library/8f2f5x2e.aspx)です。|

## <a name="related-topics"></a>関連トピック


|タイトル|説明|
|-----|-----------|
|[F# Interactive オプション](fsharp-interactive-options.md)|F# インタープリター (fsi.exe) でサポートされるコマンド ライン オプションについて説明します。|
|[プロジェクトのプロパティのリファレンス](/visualstudio/ide/reference/project-properties-reference)|ビルド オプションを提供するプロジェクトのプロパティのページなど、プロジェクトの UI について説明します。|
