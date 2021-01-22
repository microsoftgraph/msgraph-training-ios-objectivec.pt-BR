<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0e09a-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e09a-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="0e09a-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0e09a-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="0e09a-103">Para fazer isso, você integrará a Biblioteca de Autenticação da [Microsoft (MSAL) para iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="0e09a-104">Crie um novo **arquivo de lista de** propriedades no projeto **GraphTutorial** chamado **AuthSettings.plist**.</span><span class="sxs-lookup"><span data-stu-id="0e09a-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="0e09a-105">Adicione os itens a seguir ao arquivo no **dicionário** raiz.</span><span class="sxs-lookup"><span data-stu-id="0e09a-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="0e09a-106">Chave</span><span class="sxs-lookup"><span data-stu-id="0e09a-106">Key</span></span> | <span data-ttu-id="0e09a-107">Tipo</span><span class="sxs-lookup"><span data-stu-id="0e09a-107">Type</span></span> | <span data-ttu-id="0e09a-108">Valor</span><span class="sxs-lookup"><span data-stu-id="0e09a-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="0e09a-109">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="0e09a-109">String</span></span> | <span data-ttu-id="0e09a-110">A ID do aplicativo do portal do Azure</span><span class="sxs-lookup"><span data-stu-id="0e09a-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="0e09a-111">Matriz</span><span class="sxs-lookup"><span data-stu-id="0e09a-111">Array</span></span> | <span data-ttu-id="0e09a-112">Três valores de cadeia de `User.Read` caracteres: `MailboxSettings.Read` , e `Calendars.ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="0e09a-112">Three String values: `User.Read`, `MailboxSettings.Read`, and `Calendars.ReadWrite`</span></span> |

    ![Captura de tela do arquivo AuthSettings.plist no Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="0e09a-114">Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo **AuthSettings.plist** do controle de código-fonte para evitar o vazamento inadvertida da ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="0e09a-115">Implementar a login</span><span class="sxs-lookup"><span data-stu-id="0e09a-115">Implement sign-in</span></span>

<span data-ttu-id="0e09a-116">Nesta seção, você configurará o projeto para MSAL, criará uma classe de gerenciador de autenticação e atualizará o aplicativo para entrar e sair.</span><span class="sxs-lookup"><span data-stu-id="0e09a-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="0e09a-117">Configurar o projeto para MSAL</span><span class="sxs-lookup"><span data-stu-id="0e09a-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="0e09a-118">Adicione um novo grupo de chaves aos recursos do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="0e09a-118">Add a new keychain group to your project's capabilities.</span></span>
    1. <span data-ttu-id="0e09a-119">Selecione o **projeto GraphTutorial** e, em seguida, **assinando & recursos.**</span><span class="sxs-lookup"><span data-stu-id="0e09a-119">Select the **GraphTutorial** project, then **Signing & Capabilities**.</span></span>
    1. <span data-ttu-id="0e09a-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span><span class="sxs-lookup"><span data-stu-id="0e09a-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span></span>
    1. <span data-ttu-id="0e09a-121">Adicione um grupo de chaves com o `com.microsoft.adalcache` valor.</span><span class="sxs-lookup"><span data-stu-id="0e09a-121">Add a keychain group with the value `com.microsoft.adalcache`.</span></span>

1. <span data-ttu-id="0e09a-122">Controle clique **em Info.plist** e selecione **Abrir como**, em seguida, **Código-fonte**.</span><span class="sxs-lookup"><span data-stu-id="0e09a-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="0e09a-123">Adicione o seguinte dentro do `<dict>` elemento.</span><span class="sxs-lookup"><span data-stu-id="0e09a-123">Add the following inside the `<dict>` element.</span></span>

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
      </dict>
    </array>
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauthv2</string>
        <string>msauthv3</string>
    </array>
    ```

1. <span data-ttu-id="0e09a-124">Abra **AppDelegate.m** e adicione a seguinte instrução de importação na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-124">Open **AppDelegate.m** and add the following import statement at the top of the file.</span></span>

    ```objc
    #import <MSAL/MSAL.h>
    ```

1. <span data-ttu-id="0e09a-125">Adicione a função a seguir à classe `AppDelegate`.</span><span class="sxs-lookup"><span data-stu-id="0e09a-125">Add the following function to the `AppDelegate` class.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.m" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a><span data-ttu-id="0e09a-126">Criar gerenciador de autenticação</span><span class="sxs-lookup"><span data-stu-id="0e09a-126">Create authentication manager</span></span>

1. <span data-ttu-id="0e09a-127">Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **AuthenticationManager**.</span><span class="sxs-lookup"><span data-stu-id="0e09a-127">Create a new **Cocoa Touch Class** in the **GraphTutorial** project named **AuthenticationManager**.</span></span> <span data-ttu-id="0e09a-128">Escolha **NSObject** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-128">Choose **NSObject** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="0e09a-129">Abra **AuthenticationManager.h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-129">Open **AuthenticationManager.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.h" id="AuthManagerSnippet":::

1. <span data-ttu-id="0e09a-130">Abra **AuthenticationManager.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-130">Open **AuthenticationManager.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.m" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="0e09a-131">Adicionar entrar e sair</span><span class="sxs-lookup"><span data-stu-id="0e09a-131">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="0e09a-132">Abra o **arquivo SignInViewController.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-132">Open the **SignInViewController.m** file and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.m" id="SignInViewSnippet":::

1. <span data-ttu-id="0e09a-133">Abra **WelcomeViewController.m** e adicione a `import` instrução a seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-133">Open **WelcomeViewController.m** and add the following `import` statement to the top of the file.</span></span>

    ```objc
    #import "AuthenticationManager.h"
    ```

1. <span data-ttu-id="0e09a-134">Substitua a função `signOut` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="0e09a-134">Replace the existing `signOut` function with the following.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="SignOutSnippet":::

1. <span data-ttu-id="0e09a-135">Salve suas alterações e reinicie o aplicativo no Simulador.</span><span class="sxs-lookup"><span data-stu-id="0e09a-135">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="0e09a-136">Se você entrar no aplicativo, deverá ver um token de acesso exibido na janela de saída no Xcode.</span><span class="sxs-lookup"><span data-stu-id="0e09a-136">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![Uma captura de tela da janela de saída no Xcode mostrando um token de acesso](./images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="0e09a-138">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="0e09a-138">Get user details</span></span>

<span data-ttu-id="0e09a-139">Nesta seção, você criará uma classe auxiliar para manter todas as chamadas para o Microsoft Graph e atualizar para usar essa nova classe para obter o usuário `WelcomeViewController` conectado.</span><span class="sxs-lookup"><span data-stu-id="0e09a-139">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="0e09a-140">Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **GraphManager**.</span><span class="sxs-lookup"><span data-stu-id="0e09a-140">Create a new **Cocoa Touch Class** in the **GraphTutorial** project named **GraphManager**.</span></span> <span data-ttu-id="0e09a-141">Escolha **NSObject** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-141">Choose **NSObject** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="0e09a-142">Abra **GraphManager.h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-142">Open **GraphManager.h** and replace its contents with the following code.</span></span>

    ```objc
    #import <Foundation/Foundation.h>
    #import <MSGraphClientSDK/MSGraphClientSDK.h>
    #import <MSGraphClientModels/MSGraphClientModels.h>
    #import <MSGraphClientModels/MSCollection.h>
    #import "AuthenticationManager.h"

    NS_ASSUME_NONNULL_BEGIN

    typedef void (^GetMeCompletionBlock)(MSGraphUser* _Nullable user,
                                         NSError* _Nullable error);

    @interface GraphManager : NSObject

    + (id) instance;
    - (void) getMeWithCompletionBlock: (GetMeCompletionBlock) completion;

    @end

    NS_ASSUME_NONNULL_END
    ```

1. <span data-ttu-id="0e09a-143">Abra **GraphManager.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-143">Open **GraphManager.m** and replace its contents with the following code.</span></span>

    ```objc
    #import "GraphManager.h"

    @interface GraphManager()

    @property MSHTTPClient* graphClient;

    @end

    @implementation GraphManager

    + (id) instance {
        static GraphManager *singleInstance = nil;
        static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^ {
            singleInstance = [[self alloc] init];
        });

        return singleInstance;
    }

    - (id) init {
        if (self = [super init]) {
            // Create the Graph client
            self.graphClient = [MSClientFactory
                                createHTTPClientWithAuthenticationProvider:AuthenticationManager.instance];
        }

        return self;
    }

    - (void) getMeWithCompletionBlock: (GetMeCompletionBlock) completion {
        // GET /me
        NSString* meUrlString = [NSString stringWithFormat:@"%@/me?%@",
                                 MSGraphBaseURL,
                                 @"$select=displayName,mail,mailboxSettings,userPrincipalName"];
        NSURL* meUrl = [[NSURL alloc] initWithString:meUrlString];
        NSMutableURLRequest* meRequest = [[NSMutableURLRequest alloc] initWithURL:meUrl];

        MSURLSessionDataTask* meDataTask =
        [[MSURLSessionDataTask alloc]
            initWithRequest:meRequest
            client:self.graphClient
            completion:^(NSData *data, NSURLResponse *response, NSError *error) {
                if (error) {
                    completion(nil, error);
                    return;
                }

                // Deserialize the response as a user
                NSError* graphError;
                MSGraphUser* user = [[MSGraphUser alloc] initWithData:data error:&graphError];

                if (graphError) {
                    completion(nil, graphError);
                } else {
                    completion(user, nil);
                }
            }];

        // Execute the request
        [meDataTask execute];
    }

    @end
    ```

1. <span data-ttu-id="0e09a-144">Abra **WelcomeViewController.m** e adicione as instruções a `#import` seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="0e09a-144">Open **WelcomeViewController.m** and add the following `#import` statements at the top of the file.</span></span>

    ```objc
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>
    ```

1. <span data-ttu-id="0e09a-145">Adicione a propriedade a seguir à declaração `WelcomeViewController` da interface.</span><span class="sxs-lookup"><span data-stu-id="0e09a-145">Add the following property to the `WelcomeViewController` interface declaration.</span></span>

    ```objc
    @property SpinnerViewController* spinner;
    ```

1. <span data-ttu-id="0e09a-146">Substitua o existente `viewDidLoad` pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e09a-146">Replace the existing `viewDidLoad` with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="ViewDidLoadSnippet":::

<span data-ttu-id="0e09a-147">Se você salvar suas alterações e reiniciar o aplicativo agora, depois de entrar na interface do usuário for atualizado com o nome de exibição e o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="0e09a-147">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
