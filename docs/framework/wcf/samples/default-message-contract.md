---
title: 既定のメッセージ コントラクト
ms.date: 03/30/2017
helpviewer_keywords:
- Message Contract
ms.assetid: 5a200b78-1a46-4104-b7fb-da6dbab33893
ms.openlocfilehash: 29bfb88d638f68b2f9a3ba983a5c38fe49edb9a4
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33504186"
---
# <a name="default-message-contract"></a>既定のメッセージ コントラクト
既定のメッセージ コントラクトのサンプルでは、ユーザー定義のカスタム メッセージをサービス操作に渡したり、サービス操作から渡されたりするサービスを示します。 このサンプルがに基づいて、[作業の開始](../../../../docs/framework/wcf/samples/getting-started-sample.md)に型指定されたサービスとしての電卓インターフェイスを実装します。 加算、減算、乗算、および除算に使用するための個々 のサービス操作ではなく、[作業の開始](../../../../docs/framework/wcf/samples/getting-started-sample.md)、このサンプルには、オペランドと演算子の両方を格納して返すカスタム メッセージが渡されます。算術演算の結果の結果。  
  
 クライアントはコンソール プログラム (.exe) であり、サービス ライブラリはインターネット インフォメーション サービス (IIS) によってホストされます。 クライアント アクティビティは、コンソール ウィンドウに表示されます。  
  
> [!NOTE]
>  このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサービスでは、型 `MyMessage` のカスタム メッセージを受け入れて返す、1 つのサービス操作が定義されます。 このサンプルでは要求および応答のメッセージの型は同じですが、もちろん、メッセージ コントラクトは必要に応じて変えることができます。  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract(Action="http://test/MyMessage_action",  
                  ReplyAction="http://test/MyMessage_action")]  
    MyMessage Calculate(MyMessage request);  
}  
```  
  
 カスタム メッセージ `MyMessage` は、<xref:System.ServiceModel.MessageContractAttribute>、<xref:System.ServiceModel.MessageHeaderAttribute>、および <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性が付いたクラスで定義されます。 このサンプルで使用するのは 3 番目のコンストラクターけです。 メッセージ コントラクトを使用すると、SOAP メッセージを完全に制御できます。 このサンプルでは、<xref:System.ServiceModel.MessageHeaderAttribute> 属性を使用して SOAP ヘッダーに `Operation` を格納します。 オペランド `N1`、`N2`、および `Result` は SOAP 本文内に示されます。これらのオペランドには <xref:System.ServiceModel.MessageBodyMemberAttribute> 属性が適用されているためです。  
  
```  
[MessageContract]  
public class MyMessage  
{  
    private string operation;  
    private double n1;  
    private double n2;  
    private double result;  
  
    //Constructor - create an empty message.  
  
    public MyMessage() {}  
  
    //Constructor - create a message and populate its members.  
  
    public MyMessage(double n1, double n2, string operation,   
                     double result)  
    {  
        this.n1 = n1;  
        this.n2 = n2;  
        this.operation = operation;  
        this.result = result;  
    }  
  
    //Constructor - create a message from another message.  
  
    public MyMessage(MyMessage message)  
    {  
        this.n1 = message.n1;  
        this.n2 = message.n2;  
        this.operation = message.operation;  
        this.result = message.result;  
    }  
  
    [MessageHeader]  
    public string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [MessageBodyMember]  
    public double N1  
    {  
        get { return n1; }  
        set { n1 = value; }  
    }  
  
    [MessageBodyMember]  
    public double N2  
    {  
        get { return n2; }  
        set { n2 = value; }  
    }  
  
    [MessageBodyMember]  
    public double Result  
    {  
        get { return result; }  
        set { result = value; }  
    }  
}  
```  
  
 実装クラスには、`Calculate` サービス処理用のコードが含まれています。 `CalculateService` クラスは、オペランドと演算子を要求メッセージから取得し、要求された計算結果が含まれる応答メッセージを作成します。次のサンプル コードを参照してください。  
  
```  
// Service class which implements the service contract.  
public class CalculatorService : ICalculator  
{  
    // Perform a calculation.  
  
    public MyMessage Calculate(MyMessage request)  
    {  
        MyMessage response = new MyMessage(request);  
        switch (request.Operation)  
        {  
            case "+":  
                response.Result = request.N1 + request.N2;  
                break;  
            case "-":  
                response.Result = request.N1 - request.N2;  
                break;  
            case "*":  
                response.Result = request.N1 * request.N2;  
                break;  
            case "/":  
                response.Result = request.N1 / request.N2;  
                break;  
            default:  
                response.Result = 0.0D;  
                break;  
        }  
        return response;  
    }  
}  
```  
  
 クライアントの生成されたクライアント コードで作成された、 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)ツールです。 このツールでは、必要に応じて、生成済みのクライアント コード内にメッセージ コントラクト型が自動的に作成されます。 `/messageContract` コマンド オプションを指定すると、メッセージ コントラクトを強制的に生成できます。  
  
```  
svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" /o:client\generatedClient.cs http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 `MyMessage` メッセージを使用するクライアントを次のサンプル コードに示します。  
  
```  
// Create a client with given client endpoint configuration  
CalculatorClient client = new CalculatorClient();  
  
// Perform addition using a typed message.  
  
MyMessage request = new MyMessage();  
request.N1 = 100D;  
request.N2 = 15.99D;  
request.Operation = "+";  
MyMessage response = ((ICalculator)client).Calculate(request);  
Console.WriteLine("Add({0},{1}) = {2}", request.N1, request.N2, response.Result);  
```  
  
 サンプルを実行すると、クライアント コンソール ウィンドウに計算が表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。  
  
```  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 この時点で、ユーザー定義のカスタム メッセージがクライアントとサービスの操作間で渡されます。 メッセージ コントラクトでは、オペランドと結果はメッセージ本文内にあり、演算子はメッセージ ヘッダー内にあることが定義されました。 このメッセージ構造を監視するために、メッセージ ログ記録を設定できます。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1.  実行したことを確認してください、 [Windows Communication Foundation サンプルの 1 回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)です。  
  
2.  ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3.  1 つまたは複数コンピューター構成でサンプルを実行する手順についてで[Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)です。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合に、 [Windows Communication Foundation (WCF) および .NET Framework 4 向けの Windows Workflow Foundation (WF) サンプル](http://go.microsoft.com/fwlink/?LinkId=150780)すべて Windows Communication Foundation (WCF) をダウンロードして[!INCLUDE[wf1](../../../../includes/wf1-md.md)]サンプルです。 このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\Default`  
  
## <a name="see-also"></a>関連項目
