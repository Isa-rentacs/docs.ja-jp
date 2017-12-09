---
title: "型推論 (F#)"
description: "F# コンパイラが値、変数、パラメーター、および戻り値の型を推論する方法について説明します。"
keywords: "visual f#, f#, 関数型プログラミング"
author: cartermp
ms.author: phcart
ms.date: 05/16/2016
ms.topic: language-reference
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: 2d5fa4b1-732a-4d71-a62d-07f7ee79fe06
ms.openlocfilehash: c23954c0a0828cc7d2aae0553d37d5ee1dfbe152
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2017
---
# <a name="type-inference"></a>型推論

このトピックでは、f# コンパイラが値、変数、パラメーターおよび戻り値の型を推論する方法について説明します。

## <a name="type-inference-in-general"></a>推定の一般的な型します。
型の推論の概念は、f# の構成要素と、コンパイラには、型が推測できません確定以外の種類を指定する必要はありません。 F# は動的に型指定された言語に、f# で値が厳密に型指定された、明示的な型情報を省略することは限りません。 F# は、静的に型指定された言語が含まれ、コンパイラがコンパイル時に各構成体の固定の型を推測することを意味します。 コンパイラが各構成体の型を推測するのに十分な情報がない場合は、コードのどこかに明示的な型の注釈を追加することで、追加の型情報を通常指定してください。


## <a name="inference-of-parameter-and-return-types"></a>パラメーターと戻り値の型の推論
パラメーター リストでは、各パラメーターの型を指定するのにはありません。 まだ、f# は、静的に型指定された言語とすべての値と式が明確な型コンパイル時。 明示的に指定されていないその型の場合は、コンパイラは、コンテキストに基づいて型を推測します。 型がそれ以外の場合を指定でない場合は、ジェネリックにする推論されます。 コードでは、一貫性がない値を使用する場合このような方法ではなく 1 つ推論された型をコンパイラがエラーを報告する、値のすべての利用を満たします。

関数の戻り値の型は、関数の最後の式の型によって決定されます。

たとえば、次のコードでは、パラメーターの型で`a`と`b`、戻り値の型はすべてを推論する`int`ためリテラル`100`の種類は`int`します。

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-3/snippet301.fs)]

リテラルは、変更することによって型の推論の影響を与えることができます。 加えた場合、 `100` 、 `uint32` 、サフィックスを付加した`u`、種類の`a`、 `b`、および戻り値と推論されます`uint32`です。

影響を及ぼすことができます関数および特定の種類でのみ利用できるメソッドなどの型に制限を意味するその他の構文を使用して、型の推論します。

また、適用できます明示的な型の注釈関数またはメソッドのパラメーターまたは変数式では、次の例に示すようにします。 さまざまな制約との間の競合が発生すると、エラーが発生します。

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-3/snippet302.fs)]

関数の戻り値は、結局のところ、パラメーターの型の注釈を指定して明示的に指定できます。

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-3/snippet303.fs)]

型の注釈はパラメーターで便利です一般的なケースは、パラメーターが、オブジェクト型とメンバーを使用するときに、します。

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-3/snippet304.fs)]
    
## <a name="automatic-generalization"></a>自動ジェネリック化
関数のコードが、パラメーターの型に依存しない場合は、コンパイラはパラメーターをジェネリックと見なします。 これと呼ばれる*自動ジェネリック化*複雑さを増やすことがなくジェネリック コードを記述するための強力なを指定できます。

たとえば、次の関数は、任意の型の組に 2 つのパラメーターを結合します。

[!code-fsharp[Main](../../../samples/snippets/fsharp/lang-ref-3/snippet305.fs)]

型が推論されます。

```fsharp
'a -> 'b -> 'a * 'b
```

## <a name="additional-information"></a>追加情報
型の推論は f# 言語仕様で詳しく説明します。


## <a name="see-also"></a>関連項目
[自動ジェネリック化](generics/automatic-generalization.md)