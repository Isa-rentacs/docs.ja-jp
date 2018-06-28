---
title: 初期化のインスタンス化
ms.date: 03/30/2017
ms.assetid: 154d049f-2140-4696-b494-c7e53f6775ef
ms.openlocfilehash: ae01254760219f2b408ef9d9663c4158e2802be8
ms.sourcegitcommit: 15109844229ade1c6449f48f3834db1b26907824
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33807923"
---
# <a name="instancing-initialization"></a>初期化のインスタンス化
このサンプルを拡張、[プーリング](../../../../docs/framework/wcf/samples/pooling.md)インターフェイスを定義することでサンプル`IObjectControl`でのアクティブ化と非アクティブ化するオブジェクトの初期化をカスタマイズします。 クライアントは、オブジェクトをプールに返すメソッドや、プールに返さないメソッドを呼び出します。  
  
> [!NOTE]
>  このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
## <a name="extensibility-points"></a>拡張ポイント  
 Windows Communication Foundation (WCF) 拡張機能の作成の最初の手順では、使用する機能拡張ポイントを決定します。 WCF では、用語*EndpointDispatcher*は送信メッセージにそのメソッドから戻り値の変換にしたり、ユーザーのサービスでのメソッド呼び出しに着信メッセージを変換する実行時コンポーネントを表します. WCF サービスでは、各エンドポイントの EndpointDispatcher を作成します。  
  
 EndpointDispatcher は <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> クラスを使用して、(サービスによって送受信されるすべてのメッセージの) エンドポイント スコープ拡張を提供します。 このクラスにより、EndpointDispatcher の動作を制御するさまざまなプロパティをカスタマイズできます。 このサンプルでは、サービス クラスのインスタンスを提供するオブジェクトをポイントする <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> プロパティに焦点を当てています。  
  
## <a name="iinstanceprovider"></a>IInstanceProvider  
 WCF では、EndpointDispatcher を実装するインスタンス プロバイダーを使用して、サービス クラスのインスタンスを作成、<xref:System.ServiceModel.Dispatcher.IInstanceProvider>インターフェイスです。 このインターフェイスに含まれるメソッドは、次の 2 つのみです。  
  
-   <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A>: メッセージが到着すると、このディスパッチャは <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> メソッドを呼び出し、メッセージを処理するためのサービス クラスのインスタンスを作成します。 このメソッドの呼び出し頻度は <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティで決まります。 たとえば <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティが <xref:System.ServiceModel.InstanceContextMode.PerCall?displayProperty=nameWithType> に設定されている場合、サービス クラスの新しいインスタンスが作成され、到着する各メッセージが処理されます。したがって、<xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> はメッセージが到着するたびに呼び出されます。  
  
-   <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%2A>: サービス インスタンスがメッセージの処理を完了すると、EndpointDispatcher は <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%2A> メソッドを呼び出します。 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> メソッドと同様、このメソッドへの呼び出し頻度は <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> プロパティで決まります。  
  
## <a name="the-object-pool"></a>オブジェクト プール  
 `ObjectPoolInstanceProvider` クラスには、オブジェクト プールの実装が含まれています。 このクラスは、サービス モデルのレイヤと対話する <xref:System.ServiceModel.Dispatcher.IInstanceProvider> インターフェイスを実装しています。 EndpointDispatcher が、新しいインスタンスを作成する代わりに <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%2A> メソッドを呼び出すと、カスタム実装はメモリ内プールで既存のオブジェクトを検索します。 検索されたオブジェクトが使用可能な場合は、そのオブジェクトが返されます。 使用可能なオブジェクトがない場合、`ObjectPoolInstanceProvider` は `ActiveObjectsCount` プロパティ (プールから返されるオブジェクト数) が最大プール サイズに達しているかどうかをチェックします。 最大サイズに達していない場合は、新しいインスタンスが作成されて呼び出し元に返され、その後 `ActiveObjectsCount` がインクリメントされます。 最大サイズに達していると、構成期間中、オブジェクト作成要求がキューに置かれます。 `GetObjectFromThePool` の実装を次のサンプル コードに示します。  
  
```  
private object GetObjectFromThePool()  
{  
    bool didNotTimeout =   
       availableCount.WaitOne(creationTimeout, true);  
    if(didNotTimeout)  
    {  
         object obj = null;  
         lock (poolLock)  
        {  
             if (pool.Count != 0)  
             {  
                   obj = pool.Pop();  
                   activeObjectsCount++;  
             }  
             else if (pool.Count == 0)  
             {  
                   if (activeObjectsCount < maxPoolSize)  
                   {  
                        obj = CreateNewPoolObject();  
                        activeObjectsCount++;  
  
                        #if (DEBUG)  
                        WritePoolMessage(  
                             ResourceHelper.GetString("MsgNewObject"));  
                       #endif  
                   }                          
            }  
           idleTimer.Stop();  
      }  
     // Call the Activate method if possible.  
    if (obj is IObjectControl)  
   {  
         ((IObjectControl)obj).Activate();  
   }  
   return obj;  
}  
throw new TimeoutException(  
ResourceHelper.GetString("ExObjectCreationTimeout"));  
}  
```  
  
 カスタム `ReleaseInstance` 実装は、解放されたインスタンスをプールに戻し、`ActiveObjectsCount` 値をデクリメントします。 EndpointDispatcher はこれらのメソッドをさまざまなスレッドから呼び出すので、`ObjectPoolInstanceProvider` クラスのクラス レベル メンバへの同期アクセスが必要となります。  
  
```  
public void ReleaseInstance(InstanceContext instanceContext, object instance)  
{  
    lock (poolLock)  
    {  
        // Check whether the object can be pooled.   
        // Call the Deactivate method if possible.  
        if (instance is IObjectControl)  
        {  
            IObjectControl objectControl = (IObjectControl)instance;  
            objectControl.Deactivate();  
  
            if (objectControl.CanBePooled)  
            {  
                pool.Push(instance);  
  
                #if(DEBUG)  
                WritePoolMessage(  
                    ResourceHelper.GetString("MsgObjectPooled"));  
                #endif                          
            }  
            else  
            {  
                #if(DEBUG)  
                WritePoolMessage(  
                    ResourceHelper.GetString("MsgObjectWasNotPooled"));  
                #endif  
            }  
        }  
        else  
        {  
            pool.Push(instance);  
  
            #if(DEBUG)  
            WritePoolMessage(  
                ResourceHelper.GetString("MsgObjectPooled"));  
            #endif   
        }  
  
        activeObjectsCount--;  
  
        if (activeObjectsCount == 0)  
        {  
            idleTimer.Start();                       
        }  
    }  
  
    availableCount.Release(1);  
}  
```  
  
 `ReleaseInstance`メソッドには、*初期化のクリーンアップ*機能します。 通常は、プールにはその有効期間中に最小限の数のオブジェクトが保持されます。 ただし、使用率が非常に高くなり、プールでオブジェクトを追加作成する必要が生じて、その数が構成に指定されている上限に達する可能性があります。 プールが最終的にアクティブでなくなると、そうした過剰なオブジェクトは余分なオーバーヘッドになります。 したがって、`activeObjectsCount` がゼロに達すると、アイドル タイマが起動し、クリーンアップ サイクルがトリガされて実行されます。  
  
```  
if (activeObjectsCount == 0)  
{  
    idleTimer.Start();   
}  
```  
  
 ServiceModel のレイヤ拡張は、次の動作を使用してフックされます。  
  
-   サービスの動作 : サービス ランタイム全体のカスタマイズを実現します。  
  
-   エンドポイントの動作 : EndpointDispatcher を含む、特定のサービス エンドポイントのカスタマイズを実現します。  
  
-   コントラクトの動作 : クライアント上またはサービス上で、それぞれ <xref:System.ServiceModel.Dispatcher.ClientRuntime> クラスまたは <xref:System.ServiceModel.Dispatcher.DispatchRuntime> クラスのカスタマイズを実現します。  
  
-   操作の動作 : クライアント上またはサービス上のいずれかで、それぞれ <xref:System.ServiceModel.Dispatcher.ClientOperation> クラスまたは <xref:System.ServiceModel.Dispatcher.DispatchOperation> クラスのカスタマイズを実現します。  
  
 オブジェクト プール拡張を行うには、エンドポイントの動作またはサービスの動作のどちらかを作成します。 この例ではサービスの動作を使用します。この動作では、オブジェクト プール機能がサービスの各エンドポイントに適用されます。 サービス動作を作成するには、<xref:System.ServiceModel.Description.IServiceBehavior> インターフェイスを実装します。 ServiceModel にカスタム動作を認識させるには、次のようにいくつかの方法があります。  
  
-   カスタム属性を使用する。  
  
-   カスタム動作をサービス説明の動作コレクションに強制的に追加する。  
  
-   構成ファイルを拡張する。  
  
 このサンプルではカスタム属性を使用します。 <xref:System.ServiceModel.ServiceHost> が構築されると、サービスの種類の定義で使用されている属性が調べられ、使用可能な動作がサービス説明の動作コレクションに追加されます。  
  
 <xref:System.ServiceModel.Description.IServiceBehavior>インターフェイスには 3 つの方法: <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> `,` <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> `,`と<xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A>です。 これらのメソッドは、WCF によって呼び出されるときに、<xref:System.ServiceModel.ServiceHost>初期化しています。 最初に、<xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A?displayProperty=nameWithType> が呼び出されます。このメソッドによってサービスの不整合性を検査できます。 次に、<xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A?displayProperty=nameWithType> が呼び出されます。このメソッドは、非常に高度なシナリオでのみ必要です。 最後に、<xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A?displayProperty=nameWithType> が呼び出されます。このメソッドはランタイムを構成します。 次のパラメータは、<xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A?displayProperty=nameWithType> に渡されます。  
  
-   `Description` : このパラメータは、サービス全体のサービスの説明を提供します。 これを使用すると、サービスのエンドポイント、コントラクト、バインディング、およびサービスに関連するその他のデータに関する説明データを検査できます。  
  
-   `ServiceHostBase` : このパラメータは、現在初期化中の <xref:System.ServiceModel.ServiceHostBase> を提供します。  
  
 カスタム <xref:System.ServiceModel.Description.IServiceBehavior> 実装では、`ObjectPoolInstanceProvider` の新しいインスタンスがインスタンス化され、<xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> に関連付けられた各 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 内の <xref:System.ServiceModel.ServiceHostBase> プロパティに割り当てられます。  
  
```  
public void ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
{  
    if (enabled)  
    {  
        // Create an instance of the ObjectPoolInstanceProvider.  
        instanceProvider = new ObjectPoolInstanceProvider(description.ServiceType,  
        maxPoolSize, minPoolSize, creationTimeout);  
  
        // Assign our instance provider to Dispatch behavior in each   
        // endpoint.  
        foreach (ChannelDispatcherBase cdb in serviceHostBase.ChannelDispatchers)  
        {  
             ChannelDispatcher cd = cdb as ChannelDispatcher;  
             if (cd != null)  
             {  
                 foreach (EndpointDispatcher ed in cd.Endpoints)  
                 {  
                        ed.DispatchRuntime.InstanceProvider = instanceProvider;  
                 }  
             }  
         }  
     }  
}   
```  
  
 <xref:System.ServiceModel.Description.IServiceBehavior> 実装のほかにも、`ObjectPoolingAttribute` クラスには属性引数を使用してオブジェクト プールをカスタマイズするいくつかのメンバがあります。 こうしたメンバには `MaxSize`、`MinSize`、`Enabled`、`CreationTimeout` などがあり、.NET Enterprise Services で提供されるオブジェクト プール機能のセットに一致します。  
  
 オブジェクト プールの動作は今すぐを新しく作成されたカスタムのサービスの実装に注釈を付ける WCF サービスに追加された`ObjectPooling`属性。  
  
```  
[ObjectPooling(MaxSize=1024, MinSize=10, CreationTimeout=30000]      
public class PoolService : IPoolService  
{  
  // …  
}  
```  
  
## <a name="hooking-activation-and-deactivation"></a>アクティブ化と非アクティブ化のフック  
 オブジェクト プールの主な目的は、比較的負荷のかかる作成および初期化を伴う有効期間の短いオブジェクトを最適化することです。 そのため、オブジェクト プールが適切に使用された場合は、アプリケーションのパフォーマンスを大幅に向上させることができます。 オブジェクトはプールから返されるので、コンストラクター呼び出されるのは 1 回だけです。 ただし一部のアプリケーションでは、単一のコンテキスト内で使用されるリソースを初期化してクリーンアップできるようにするために、ある一定のレベルの制御が必要になります。 たとえば、一連の計算に使用されているオブジェクトは、次の計算を実行する前に、そのオブジェクトのプライベート フィールドをリセットできます。 Enterprise Services では、オブジェクト開発者が `Activate` ベース クラスの `Deactivate` メソッドおよび <xref:System.EnterpriseServices.ServicedComponent> メソッドをオーバーライドすることにより、コンテキスト固有のこの種の初期化が実現されます。  
  
 オブジェクト プールは、オブジェクトがプールから返される直前に `Activate` メソッドを呼び出します。 オブジェクトがプールに返される際には `Deactivate` メソッドが呼び出されます。 <xref:System.EnterpriseServices.ServicedComponent> ベース クラスには、`boolean` という `CanBePooled` プロパティもあります。このプロパティを使用すると、オブジェクトをさらにプールできるかどうかをプールに通知できます。  
  
 このサンプルではこの機能に似た動作が行われるように、前述のメンバを持つパブリック インターフェイス (`IObjectControl`) を宣言しています。 このインターフェイスは、コンテキスト固有の初期化を提供するためのサービス クラスによって実装されます。 <xref:System.ServiceModel.Dispatcher.IInstanceProvider> の実装は、これらの要件を満たすように変更する必要があります。 ここで、毎回オブジェクトを取得するを呼び出して、`GetInstance`メソッド、オブジェクトが実装されているかどうか確認する必要があります`IObjectControl.`場合は、呼び出す必要があります、`Activate`メソッド適切にします。  
  
```  
if (obj is IObjectControl)  
{  
    ((IObjectControl)obj).Activate();  
}  
```  
  
 オブジェクトをプールに返すときには、そのオブジェクトをプールに追加し直す前に、`CanBePooled` プロパティのチェックが必要です。  
  
```  
if (instance is IObjectControl)  
{  
    IObjectControl objectControl = (IObjectControl)instance;  
    objectControl.Deactivate();  
    if (objectControl.CanBePooled)  
    {  
       pool.Push(instance);  
    }  
}  
```  
  
 オブジェクトをプールできるかどうかはサービス開発者が決定できるので、指定時間におけるプール内のオブジェクト数は、最低サイズ未満になる場合があります。 したがって、オブジェクト数が最低レベル未満になっているかどうかをチェックし、クリーンアップ手順において必要な初期化を実行する必要があります。  
  
```  
// Remove the surplus objects.  
if (pool.Count > minPoolSize)  
{  
  // Clean the surplus objects.  
}                      
else if (pool.Count < minPoolSize)  
{  
  // Reinitialize the missing objects.  
  while(pool.Count != minPoolSize)  
  {  
    pool.Push(CreateNewPoolObject());  
  }  
}  
```  
  
 このサンプルを実行すると、操作要求と応答がサービスとクライアントの両方のコンソール ウィンドウに表示されます。 どちらかのコンソールで Enter キーを押すと、サービスとクライアントがどちらもシャットダウンされます。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1.  実行したことを確認してください、 [Windows Communication Foundation サンプルの 1 回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)です。  
  
2.  指示に従って、ソリューションをビルドする[Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)です。  
  
3.  1 つまたは複数コンピューター構成でサンプルを実行する手順についてで[Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)です。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合に、 [Windows Communication Foundation (WCF) および .NET Framework 4 向けの Windows Workflow Foundation (WF) サンプル](http://go.microsoft.com/fwlink/?LinkId=150780)すべて Windows Communication Foundation (WCF) をダウンロードして[!INCLUDE[wf1](../../../../includes/wf1-md.md)]サンプルです。 このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Initialization`  
  
## <a name="see-also"></a>関連項目
