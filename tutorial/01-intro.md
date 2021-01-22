<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d0034-101">Este tutorial ensina a criar um aplicativo React Native que usa a API do Microsoft Graph para recuperar informações de calendário para um usuário.</span><span class="sxs-lookup"><span data-stu-id="d0034-101">This tutorial teaches you how to build an React Native app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="d0034-102">Se preferir apenas baixar o tutorial concluído, você pode baixar ou clonar o repositório [do GitHub.](https://github.com/microsoftgraph/msgraph-training-ios-objectivec)</span><span class="sxs-lookup"><span data-stu-id="d0034-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-objectivec).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0034-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="d0034-103">Prerequisites</span></span>

<span data-ttu-id="d0034-104">Antes de começar este tutorial, você deve ter o seguinte instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d0034-104">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="d0034-105">Xcode</span><span class="sxs-lookup"><span data-stu-id="d0034-105">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="d0034-106">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="d0034-106">CocoaPods</span></span>](https://cocoapods.org)

<span data-ttu-id="d0034-107">Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta de trabalho ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d0034-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="d0034-108">Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="d0034-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="d0034-109">Você pode [se inscrever para uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="d0034-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="d0034-110">Você pode se inscrever no Programa para Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="d0034-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d0034-111">Este tutorial foi escrito usando o Xcode versão 12.3 e o CocoaPods versão 1.10.1.</span><span class="sxs-lookup"><span data-stu-id="d0034-111">This tutorial was written using Xcode version 12.3 and CocoaPods version 1.10.1.</span></span> <span data-ttu-id="d0034-112">As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="d0034-112">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="d0034-113">Comentários</span><span class="sxs-lookup"><span data-stu-id="d0034-113">Feedback</span></span>

<span data-ttu-id="d0034-114">Faça comentários sobre este tutorial no repositório [do GitHub.](https://github.com/microsoftgraph/msgraph-training-ios-objectivec)</span><span class="sxs-lookup"><span data-stu-id="d0034-114">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-objectivec).</span></span>
