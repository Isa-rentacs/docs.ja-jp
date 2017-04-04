---
title: "グローバル名前空間 (Visual Basic) (LINQ to XML) の使用 |Microsoft ドキュメント"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: 0a8064d5-e02f-4315-ad48-6deaa443a2f0
caps.latest.revision: 3
author: stevehoag
ms.author: shoag
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: e20d0c0b670d63ebe2be137a48dc1a1a9a861b9f
ms.lasthandoff: 03/13/2017

---
# <a name="working-with-global-namespaces-visual-basic-linq-to-xml"></a>グローバル名前空間の使用 (Visual Basic) (LINQ to XML)
使用して、XML 名前空間を宣言できる Visual Basic で XML リテラルの主な機能の&1; つは、`Imports`ステートメントです。 この機能を使用することで、プレフィックスを使用する XML 名前空間または既定の XML 名前空間を宣言できます。  
  
 この機能は&2; つの状況で役立ちます。 1 つは、XML リテラルで宣言された名前空間が組み込み式に引き継がれない場合です。 グローバル名前空間を宣言すると、名前空間を伴う組み込み式を使用する必要がある場合の作業が軽減されます。 もう&1; つは、名前空間と XML プロパティを併用するためにグローバル名前空間を宣言する必要がある場合です。  
  
 グローバル名前空間はプロジェクト レベルで宣言できます。 また、モジュール レベルでもグローバル名前空間を宣言できます。その場合、プロジェクト レベルのグローバル名前空間はオーバーライドされます。 最終的に、グローバル名前空間は XML リテラルでオーバーライドできます。  
  
 グローバルに宣言された名前空間に含まれる XML リテラルまたは XML プロパティを使用する場合は、Visual Studio で XML リテラルまたはプロパティにカーソルを合わせることで、それらの展開名が表示されます。 拡張名はツールヒントに表示されます。  
  
 取得できます、<xref:System.Xml.Linq.XNamespace>オブジェクトを使用して、グローバル名前空間に対応する、`GetXmlNamespace`メソッド</xref:System.Xml.Linq.XNamespace>。  
  
## <a name="examples-of-global-namespaces"></a>グローバル名前空間の例  
 次の例では、既定のグローバル名前空間を宣言を使用して、`Imports`ステートメント、および、初期化するために、XML リテラルを使用して、<xref:System.Xml.Linq.XElement>その名前空間内のオブジェクト:</xref:System.Xml.Linq.XElement>  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <Root/>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com" />  
```  
  
 次の例では、プレフィックスを持つグローバル名前空間を宣言し、XML リテラルを使用して要素を初期化します。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <aw:Root/>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com" />  
```  
  
## <a name="global-namespaces-and-embedded-expressions"></a>グローバル名前空間と組み込み式  
 XML リテラルで宣言される名前空間は、組み込み式に引き継がれません。 次の例では、既定の名前空間を宣言しています。 さらに、`Child` 要素に組み込み式を使用しています。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <%= <Child/> %>  
    </Root>  
Console.WriteLine(root)  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child xmlns="" />  
</Root>  
```  
  
 このように、結果の XML では、`Child` 要素がどの名前空間にも属さないように、既定の名前空間の宣言が含まれています。  
  
 次のように、組み込み式で名前空間を再宣言できます。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <%= <Child xmlns="http://www.adventure-works.com"/> %>  
    </Root>  
Console.WriteLine(root)  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child xmlns="http://www.adventure-works.com" />  
</Root>  
```  
  
 ただし、この方法は既定のグローバル名前空間よりも使い方が複雑であり、既定のグローバル名前空間を使用するアプローチの方がより適切です。 既定のグローバル名前空間を使用すると、名前空間を宣言せずに XML リテラルを使用できます。 結果の XML は、グローバルに宣言された既定の名前空間に含まれることになります。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <Root>  
                                   <%= <Child/> %>  
                               </Root>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child />  
</Root>  
```  
  
## <a name="using-namespaces-with-xml-properties"></a>名前空間と XML プロパティの併用  
 名前空間に含まれている XML ツリーを操作する場合に XML プロパティを使用するときは、XML プロパティが正しい名前空間に含まれるように、グローバル名前空間を使用する必要があります。 次の例では、名前空間内で XML ツリーを宣言しています。 さらに、`Child` 要素の数を出力しています。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <Child>content</Child>  
    </Root>  
Console.WriteLine(root.<Child>.Count())  
```  
  
 この例は `Child` 要素が存在しないことを示しています。 生成される出力は次のとおりです。  
  
```  
0  
```  
  
 ただし、既定のグローバル名前空間を宣言すると、XML リテラルと XML プロパティの両方が既定のグローバル名前空間に含まれます。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child>content</Child>  
            </Root>  
        Console.WriteLine(root.<Child>.Count())  
    End Sub  
End Module  
```  
  
 この例は、`Child` 要素が&1; つ存在することを示しています。 生成される出力は次のとおりです。  
  
```  
1  
```  
  
 プレフィックスを持つグローバル名前空間を宣言すると、そのプレフィックスを XML リテラルと XML プロパティの両方で使用できます。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <aw:Child>content</aw:Child>  
            </aw:Root>  
        Console.WriteLine(root.<aw:Child>.Count())  
    End Sub  
End Module  
```  
  
## <a name="xnamespace-and-global-namespaces"></a>XNamespace とグローバル名前空間  
 取得できます、<xref:System.Xml.Linq.XNamespace>オブジェクトを使用して、`GetXmlNamespace`メソッド:</xref:System.Xml.Linq.XNamespace>  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <aw:Root/>  
        Dim xn As XNamespace = GetXmlNamespace(aw)  
        Console.WriteLine(xn)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```  
http://www.adventure-works.com  
```  
  
## <a name="see-also"></a>関連項目  
 [XML 名前空間 (Visual Basic) の使用](../../../../visual-basic/programming-guide/concepts/linq/working-with-xml-namespaces.md)