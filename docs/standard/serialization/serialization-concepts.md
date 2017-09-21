---
title: "シリアル化の概念"
ms.date: 08/07/2017
ms.prod: .net
ms.topic: article
ms.assetid: e1ff4740-20a1-4c76-a8ad-d857db307054
caps.latest.revision: 4
author: Erikre
ms.author: erikre
manager: erikre
ms.translationtype: HT
ms.sourcegitcommit: 717bcb6f9f72a728d77e2847096ea558a9c50902
ms.openlocfilehash: 82349611fe127da46bed8998ac883c10c5164cd3
ms.contentlocale: ja-jp
ms.lasthandoff: 08/21/2017

---
# <a name="serialization-concepts"></a>シリアル化の概念
シリアル化が必要となる理由について考えてみます。 最も重要な理由として、オブジェクトの状態をストレージ メディアに保持し、後の段階で同一コピーを再作成できるようにすることと、アプリケーション ドメイン間でオブジェクトを値渡しで送信することの 2 つが挙げられます。 たとえば、シリアル化は ASP.NET でのセッション状態を保存したり、オブジェクトを Windows フォームのクリップボードにコピーしたりするために使用されます。 また、リモート処理でオブジェクトを 1 つのアプリケーション ドメインから別のアプリケーション ドメインに値渡しするためにも使用されます。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

## <a name="persistent-storage"></a>永続ストレージ
オブジェクトのフィールドの値をディスクに格納しておき、後でこのデータを取得する必要が生じることはよくあります。 これはシリアル化を使用しなくても簡単に実現できますが、シリアル化を使用しない方法は煩雑でエラーの原因となることが多く、オブジェクトの階層を追跡する必要がある場合にはさらに複雑になります。 何千ものオブジェクトを含む大規模なビジネス アプリケーションを作成し、それぞれのオブジェクトのフィールドやプロパティをディスクに保存したり、ディスクから復元したりするコードを記述することを想像してみてください。 シリアル化は、この目的を実現するための便利なメカニズムを提供します。

共通言語ランタイムは、オブジェクトがメモリに格納される方法を管理し、[リフレクション](../../../docs/framework/reflection-and-codedom/reflection.md)を使用して自動シリアル化のメカニズムを提供します。 オブジェクトをシリアル化すると、クラスの名前、アセンブリ、およびそのクラス インスタンスのすべてのデータ メンバーがストレージに書き込まれます。 オブジェクトのメンバー変数には、他のインスタンスへの参照が格納されていることがよくあります。 クラスをシリアル化するとき、シリアル化エンジンは、既にシリアル化されている参照先オブジェクトを追跡し、同じオブジェクトが複数回シリアル化されないようにします。 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] に用意されているシリアル化アーキテクチャは、オブジェクト グラフおよび循環参照を自動的に正しく処理します。 オブジェクト グラフに対する唯一の要件は、シリアル化されたオブジェクトによって参照されるすべてのオブジェクトを、`Serializable` としてもマークすることです (詳細については、「[基本的なシリアル化](basic-serialization.md)」を参照)。 Serializable としてマークしないと、マークされていないオブジェクトをシリアライザーがシリアル化しようとしたときに例外がスローされます。

シリアル化されたクラスを逆シリアル化すると、そのクラスが再作成され、すべてのデータ メンバーの値は自動的に復元されます。

## <a name="marshal-by-value"></a>値渡しによるマーシャリング
オブジェクトは、作成されたアプリケーション ドメイン内でのみ有効です。 オブジェクトをパラメーターとして渡したり、結果として返したりする処理を試行すると、そのオブジェクトが `MarshalByRefObject` から派生しているか、または `Serializable` としてマークされている場合を除いて、その試行は失敗します。 `Serializable` としてマークされていると、オブジェクトは自動的にシリアル化され、アプリケーション ドメイン間で転送されます。その後、逆シリアル化され、転送先のアプリケーション ドメインにそのオブジェクトの同一コピーが作成されます。 この処理は一般に、値渡しによるマーシャリングと呼ばれます。
 
オブジェクトが `MarshalByRefObject` から派生している場合は、そのオブジェクト自体ではなく、オブジェクト参照がアプリケーション ドメイン間で渡されます。 `MarshalByRefObject` から派生したオブジェクトを `Serializable` としてマークすることもできます。 このオブジェクトがリモート処理で使用されると、サロゲート セレクター (`SurrogateSelector`) によって事前構成された、シリアル化を実行するフォーマッタがシリアル化プロセスを制御し、`MarshalByRefObject` から派生したすべてのオブジェクトをプロキシに置き換えます。 `SurrogateSelector` を使用しない場合、シリアル化アーキテクチャは「[シリアル化プロセスの手順](steps-in-the-serialization-process.md)」で説明されている標準のシリアル化のルールに従います。  

## <a name="related-sections"></a>関連項目  
 [バイナリ シリアル化](../../../docs/standard/serialization/binary-serialization.md)  
 共通言語ランタイムに付属しているバイナル シリアル化機構について説明します。  
  
 [リモート オブジェクト](http://msdn.microsoft.com/en-us/515686e6-0a8d-42f7-8188-73abede57c58)  
 .NET Framework でリモート通信に利用できるさまざまな通信方法について説明します。  
  
 [XML シリアル化および SOAP シリアル化](../../../docs/standard/serialization/xml-and-soap-serialization.md)  
 共通言語ランタイムに付属している XML シリアル化および SOAP シリアル化機構について説明します。
