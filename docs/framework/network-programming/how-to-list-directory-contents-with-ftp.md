---
title: '方法: FTP でディレクトリの内容を一覧表示する'
ms.date: 03/30/2017
ms.assetid: 130c64c9-7b7f-4672-9b3b-d946bd2616c5
author: mcleblanc
ms.author: markl
manager: markl
ms.openlocfilehash: 12351b06dc7d03971f9ce70f36110b8b6d672fd5
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33394580"
---
# <a name="how-to-list-directory-contents-with-ftp"></a>方法: FTP でディレクトリの内容を一覧表示する
このサンプルでは、FTP サーバーのディレクトリの内容を一覧表示する方法を示します。  
  
## <a name="example"></a>例  
  
```csharp  
using System;  
using System.IO;  
using System.Net;  
using System.Text;  
  
namespace Examples.System.Net  
{  
    public class WebRequestGetExample  
    {  
        public static void Main ()  
        {  
            // Get the object used to communicate with the server.  
            FtpWebRequest request = (FtpWebRequest)WebRequest.Create("ftp://www.contoso.com/");  
            request.Method = WebRequestMethods.Ftp.ListDirectoryDetails;  
  
            // This example assumes the FTP site uses anonymous logon.  
            request.Credentials = new NetworkCredential ("anonymous","janeDoe@contoso.com");  
  
            FtpWebResponse response = (FtpWebResponse)request.GetResponse();  
  
            Stream responseStream = response.GetResponseStream();  
            StreamReader reader = new StreamReader(responseStream);  
            Console.WriteLine(reader.ReadToEnd());  
  
            Console.WriteLine("Directory List Complete, status {0}", response.StatusDescription);  
  
            reader.Close();  
            response.Close();  
        }  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
-   **System.Net** 名前空間への参照。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ
