<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ff1ef-101">Neste exercício, você criará um novo aplicativo nativo do Azure AD usando o centro de administração do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="ff1ef-102">Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="ff1ef-103">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="ff1ef-104">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="ff1ef-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="ff1ef-105">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-105">Select **New registration**.</span></span> <span data-ttu-id="ff1ef-106">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ff1ef-107">Defina **Nome** para `iOS Objective-C Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-107">Set **Name** to `iOS Objective-C Graph Tutorial`.</span></span>
    - <span data-ttu-id="ff1ef-108">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ff1ef-109">Em **URI de redirecionamento**, altere o menu suspenso para **cliente público (Desktop & móvel)** e defina `msauth.YOUR_BUNDLE_ID://auth`o valor `YOUR_BUNDLE_ID` como, substituindo com a ID do pacote para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `msauth.YOUR_BUNDLE_ID://auth`, replacing `YOUR_BUNDLE_ID` with the bundle ID for your app.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. <span data-ttu-id="ff1ef-111">Selecione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-111">Select **Register**.</span></span> <span data-ttu-id="ff1ef-112">Na página **tutorial do gráfico de objetivo do IOS-C** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="ff1ef-112">On the **iOS Objective-C Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)
