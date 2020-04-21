# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="0997d-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="0997d-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0997d-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0997d-102">Prerequisites</span></span>

<span data-ttu-id="0997d-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="0997d-103">To run the completed project in this folder, you need the following:</span></span>

- [<span data-ttu-id="0997d-104">Xcode</span><span class="sxs-lookup"><span data-stu-id="0997d-104">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="0997d-105">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="0997d-105">CocoaPods</span></span>](https://cocoapods.org)
- <span data-ttu-id="0997d-106">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0997d-106">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

> <span data-ttu-id="0997d-107">**Observação:** Este tutorial foi escrito usando o Xcode versão 11,4 e o CocoaPods versão 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="0997d-107">**Note:** This tutorial was written using Xcode version 11.4 and CocoaPods version 1.9.1.</span></span> <span data-ttu-id="0997d-108">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="0997d-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

<span data-ttu-id="0997d-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="0997d-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0997d-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="0997d-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0997d-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="0997d-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="0997d-112">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0997d-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="0997d-113">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0997d-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="0997d-114">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="0997d-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="0997d-115">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="0997d-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="0997d-116">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="0997d-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="0997d-117">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="0997d-117">Select **New registration**.</span></span> <span data-ttu-id="0997d-118">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="0997d-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="0997d-119">Defina **Nome** para `iOS Objective-C Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="0997d-119">Set **Name** to `iOS Objective-C Graph Tutorial`.</span></span>
    - <span data-ttu-id="0997d-120">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="0997d-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="0997d-121">Deixe o **URI de Redirecionamento** vazio.</span><span class="sxs-lookup"><span data-stu-id="0997d-121">Leave **Redirect URI** empty.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="0997d-123">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="0997d-123">Choose **Register**.</span></span> <span data-ttu-id="0997d-124">Na página **tutorial do gráfico de objetivo do IOS-C** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="0997d-124">On the **iOS Objective-C Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="0997d-126">Selecione **Autenticação** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="0997d-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="0997d-127">Selecione **Adicionar uma plataforma**e **Ios/MacOS**.</span><span class="sxs-lookup"><span data-stu-id="0997d-127">Select **Add a platform**, then **iOS / macOS**.</span></span>

1. <span data-ttu-id="0997d-128">Insira a ID de pacote do aplicativo e selecione **Configurar**e, em seguida, selecione **concluído**.</span><span class="sxs-lookup"><span data-stu-id="0997d-128">Enter your app's Bundle ID and select **Configure**, then select **Done**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="0997d-129">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="0997d-129">Configure the sample</span></span>

1. <span data-ttu-id="0997d-130">Renomear `AuthSettings.plist.example` o `AuthSettings.plist`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="0997d-130">Rename the `AuthSettings.plist.example` file to `AuthSettings.plist`.</span></span>
1. <span data-ttu-id="0997d-131">Edite `AuthSettings.plist` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="0997d-131">Edit the `AuthSettings.plist` file and make the following changes.</span></span>
    1. <span data-ttu-id="0997d-132">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0997d-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="0997d-133">Abra o terminal no diretório **GraphTutorial** e execute o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="0997d-133">Open Terminal in the **GraphTutorial** directory and run the following command.</span></span>

    ```Shell
    pod install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="0997d-134">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="0997d-134">Run the sample</span></span>

<span data-ttu-id="0997d-135">No Xcode, selecione o menu **produto** e, em seguida, selecione **executar**.</span><span class="sxs-lookup"><span data-stu-id="0997d-135">In Xcode, select the **Product** menu, then select **Run**.</span></span>
