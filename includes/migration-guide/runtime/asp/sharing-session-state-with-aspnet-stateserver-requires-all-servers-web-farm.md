### <a name="sharing-session-state-with-aspnet-stateserver-requires-all-servers-in-the-web-farm-to-use-the-same-net-framework-version"></a><span data-ttu-id="939f0-101">Asp.Net StateServer とセッション状態を共有するには、Web ファーム内のすべてのサーバーが同じ .NET Framework バージョンを使用する必要があります</span><span class="sxs-lookup"><span data-stu-id="939f0-101">Sharing session state with Asp.Net StateServer requires all servers in the web farm to use the same .NET Framework version</span></span>

|   |   |
|---|---|
|<span data-ttu-id="939f0-102">説明</span><span class="sxs-lookup"><span data-stu-id="939f0-102">Details</span></span>|<span data-ttu-id="939f0-103"><xref:System.Web.SessionState.SessionStateMode.StateServer?displayProperty=name> セッション状態を有効にする場合、状態を正しく共有するには、指定の Web ファーム内のすべてのサーバーが同じバージョンの .NET Framework を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="939f0-103">When enabling <xref:System.Web.SessionState.SessionStateMode.StateServer?displayProperty=name> session state, all of the servers in the given web farm must use the same version of the .NET Framework in order for state to be properly shared.</span></span>|
|<span data-ttu-id="939f0-104">提案される解決策</span><span class="sxs-lookup"><span data-stu-id="939f0-104">Suggestion</span></span>|<span data-ttu-id="939f0-105">同時に状態を共有する Web サーバー上で必ず .NET Framework バージョンのアップグレードを行います。</span><span class="sxs-lookup"><span data-stu-id="939f0-105">Be sure to upgrade .NET Framework versions on web servers that share state at the same time.</span></span>|
|<span data-ttu-id="939f0-106">スコープ</span><span class="sxs-lookup"><span data-stu-id="939f0-106">Scope</span></span>|<span data-ttu-id="939f0-107">エッジ</span><span class="sxs-lookup"><span data-stu-id="939f0-107">Edge</span></span>|
|<span data-ttu-id="939f0-108">Version</span><span class="sxs-lookup"><span data-stu-id="939f0-108">Version</span></span>|<span data-ttu-id="939f0-109">4.5</span><span class="sxs-lookup"><span data-stu-id="939f0-109">4.5</span></span>|
|<span data-ttu-id="939f0-110">型</span><span class="sxs-lookup"><span data-stu-id="939f0-110">Type</span></span>|<span data-ttu-id="939f0-111">ランタイム</span><span class="sxs-lookup"><span data-stu-id="939f0-111">Runtime</span></span>|
|<span data-ttu-id="939f0-112">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="939f0-112">Affected APIs</span></span>|<ul><li><xref:System.Web.SessionState.SessionStateMode.StateServer?displayProperty=nameWithType></li></ul>|
