---
title: エラー メッセージ (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- errors [Visual Basic]
- error messages
- trappable errors
- errors [Visual Basic], trappable
ms.assetid: f2dda05b-baef-41f5-8bb1-598bd7cf239f
ms.openlocfilehash: c326b781222429d68ec4385d95507a6ba99eafcb
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33590135"
---
# <a name="error-messages-visual-basic"></a>エラー メッセージ (Visual Basic)
Visual Basic アプリケーションを作成、コンパイル、実行する際は、次の種類のエラーが発生する可能性があります。  
  
1.  デザイン時エラー: Visual Studio でアプリケーションを作成するときに発生します。  
  
2.  コンパイル時エラー: Visual Studio またはコマンド プロンプトでアプリケーションをコンパイルするときに発生します。  
  
3.  実行時エラー: Visual Studio でアプリケーションまたはスタンドアロンの実行可能ファイルを実行するときに発生します。  
  
 特定のエラーのトラブルシューティング方法については、「[Visual Basic プログラマのための追加リソース](../../../visual-basic/getting-started/additional-resources.md)」を参照してください。  
  
## <a name="run-time-errors"></a>実行時エラー  
 Visual Basic アプリケーションでは、システムを実行できないアクションを実行しようとすると、実行時エラーが発生し、Visual Basic をスロー、`Exception`オブジェクト。 Visual Basic には、すべてのデータのカスタム エラーが生成される型を含む`Exception`を使用して、オブジェクト、`Throw`ステートメントです。 アプリケーションは、キャッチされた例外のエラー番号とメッセージを表示して、エラーを識別できます。 エラーがキャッチされない場合、アプリケーションは終了します。  
  
 実行時エラーはコードでトラップして調べることができます。 エラーが発生するコードを `Try` ブロックで囲むと、スローされたエラーを対応する `Catch` ブロック内でキャッチできます。 実行時にエラーをトラップしてコードで対処する方法については、「[Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)」を参照してください。  
  
## <a name="compile-time-errors"></a>コンパイル時エラー  
 Visual Basic コンパイラがコード内で問題を検出すると、コンパイル時エラーが発生します。 コード エディターでは、エラーの原因となったコード行の下に波線が表示されるため、簡単にその行を特定できます。 波線をポイントするか**エラー一覧**を開くと、エラー メッセージが表示されます。エラー一覧には他のメッセージも表示されます。  
  
 識別子の下に波線があり、右端の文字の下に短い下線が表示されている場合は、クラス、コンストラクター、メソッド、プロパティ、フィールド、または列挙型のスタブを生成できます。 詳細については、「[Generate From Usage](/visualstudio/ide/visual-csharp-intellisense#generate-from-usage)」(使用法から生成) を参照してください。
  
 Visual Basic コンパイラからのエラーを解決することにより、高速に実行されるバグの少ないコードを作成できます。 これらの警告では、アプリケーションの実行時にエラーを発生させるコードが示されます。 たとえば、ユーザーが、割り当てが行われていないオブジェクト変数のメンバーを呼び出そうとしたり、戻り値を設定せずに関数から戻ろうとしたり、例外をキャッチするロジックにエラーがある `Try` ブロックを実行しようとしたりすると、コンパイラは警告を表示します。 警告の表示/非表示を切り替える方法など、警告の詳細については、「[Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。
