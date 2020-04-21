<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2e789-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e789-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="2e789-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2e789-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="2e789-103">Para fazer isso, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para IOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e789-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="2e789-104">Crie um novo arquivo de **lista de propriedades** no projeto do **GraphTutorial** chamado **AuthSettings. plist**.</span><span class="sxs-lookup"><span data-stu-id="2e789-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="2e789-105">Adicione os seguintes itens ao arquivo no dicionário **raiz** .</span><span class="sxs-lookup"><span data-stu-id="2e789-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="2e789-106">Chave</span><span class="sxs-lookup"><span data-stu-id="2e789-106">Key</span></span> | <span data-ttu-id="2e789-107">Tipo</span><span class="sxs-lookup"><span data-stu-id="2e789-107">Type</span></span> | <span data-ttu-id="2e789-108">Valor</span><span class="sxs-lookup"><span data-stu-id="2e789-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="2e789-109">String</span><span class="sxs-lookup"><span data-stu-id="2e789-109">String</span></span> | <span data-ttu-id="2e789-110">A ID do aplicativo do portal do Azure</span><span class="sxs-lookup"><span data-stu-id="2e789-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="2e789-111">Matriz</span><span class="sxs-lookup"><span data-stu-id="2e789-111">Array</span></span> | <span data-ttu-id="2e789-112">Dois valores de cadeia `User.Read` de caracteres: e`Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="2e789-112">Two String values: `User.Read` and `Calendars.Read`</span></span> |

    ![Uma captura de tela do arquivo AuthSettings. plist no Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="2e789-114">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **AuthSettings. plist** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e789-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="2e789-115">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="2e789-115">Implement sign-in</span></span>

<span data-ttu-id="2e789-116">Nesta seção, você irá configurar o projeto para o MSAL, criar uma classe do Gerenciador de autenticação e atualizar o aplicativo para entrar e sair.</span><span class="sxs-lookup"><span data-stu-id="2e789-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="2e789-117">Configurar o Project para o MSAL</span><span class="sxs-lookup"><span data-stu-id="2e789-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="2e789-118">Adicione um novo grupo de chaves aos recursos do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="2e789-118">Add a new keychain group to your project's capabilities.</span></span>
    1. <span data-ttu-id="2e789-119">Selecione o projeto **GraphTutorial** e, em seguida, **assinaturas & recursos**.</span><span class="sxs-lookup"><span data-stu-id="2e789-119">Select the **GraphTutorial** project, then **Signing & Capabilities**.</span></span>
    1. <span data-ttu-id="2e789-120">Selecione **+ recurso**e, em seguida, clique duas vezes em **compartilhamento de chaves**.</span><span class="sxs-lookup"><span data-stu-id="2e789-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span></span>
    1. <span data-ttu-id="2e789-121">Adicione um grupo de chaves com o valor `com.microsoft.adalcache`.</span><span class="sxs-lookup"><span data-stu-id="2e789-121">Add a keychain group with the value `com.microsoft.adalcache`.</span></span>

1. <span data-ttu-id="2e789-122">Control clique **info. plist** e selecione **abrir como**e, em seguida, **código-fonte**.</span><span class="sxs-lookup"><span data-stu-id="2e789-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="2e789-123">Adicione o seguinte no `<dict>` elemento.</span><span class="sxs-lookup"><span data-stu-id="2e789-123">Add the following inside the `<dict>` element.</span></span>

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

1. <span data-ttu-id="2e789-124">Abra **AppDelegate. m** e adicione a seguinte instrução de importação na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="2e789-124">Open **AppDelegate.m** and add the following import statement at the top of the file.</span></span>

    ```objc
    #import <MSAL/MSAL.h>
    ```

1. <span data-ttu-id="2e789-125">Adicione a função a seguir à classe `AppDelegate`.</span><span class="sxs-lookup"><span data-stu-id="2e789-125">Add the following function to the `AppDelegate` class.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.m" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a><span data-ttu-id="2e789-126">Criar o Gerenciador de autenticação</span><span class="sxs-lookup"><span data-stu-id="2e789-126">Create authentication manager</span></span>

1. <span data-ttu-id="2e789-127">Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **AuthenticationManager**.</span><span class="sxs-lookup"><span data-stu-id="2e789-127">Create a new **Cocoa Touch Class** in the **GraphTutorial** project named **AuthenticationManager**.</span></span> <span data-ttu-id="2e789-128">Escolha **NSObject** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="2e789-128">Choose **NSObject** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="2e789-129">Abra **AuthenticationManager. h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-129">Open **AuthenticationManager.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.h" id="AuthManagerSnippet":::

1. <span data-ttu-id="2e789-130">Abra **AuthenticationManager. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-130">Open **AuthenticationManager.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.m" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="2e789-131">Adicionar entrada e saída</span><span class="sxs-lookup"><span data-stu-id="2e789-131">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="2e789-132">Abra o arquivo **SignInViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-132">Open the **SignInViewController.m** file and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.m" id="SignInViewSnippet":::

1. <span data-ttu-id="2e789-133">Abra **WelcomeViewController. m** e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="2e789-133">Open **WelcomeViewController.m** and add the following `import` statement to the top of the file.</span></span>

    ```objc
    #import "AuthenticationManager.h"
    ```

1. <span data-ttu-id="2e789-134">Substitua a função `signOut` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="2e789-134">Replace the existing `signOut` function with the following.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="SignOutSnippet":::

1. <span data-ttu-id="2e789-135">Salve suas alterações e reinicie o aplicativo no simulador.</span><span class="sxs-lookup"><span data-stu-id="2e789-135">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="2e789-136">Se você entrar no aplicativo, verá um token de acesso exibido na janela de saída do Xcode.</span><span class="sxs-lookup"><span data-stu-id="2e789-136">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![Uma captura de tela da janela de saída no Xcode mostrando um token de acesso](./images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="2e789-138">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="2e789-138">Get user details</span></span>

<span data-ttu-id="2e789-139">Nesta seção, você criará uma classe auxiliar para manter todas as chamadas para o Microsoft Graph e atualizará `WelcomeViewController` o para usar essa nova classe para obter o usuário conectado.</span><span class="sxs-lookup"><span data-stu-id="2e789-139">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="2e789-140">Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **graphmanager**.</span><span class="sxs-lookup"><span data-stu-id="2e789-140">Create a new **Cocoa Touch Class** in the **GraphTutorial** project named **GraphManager**.</span></span> <span data-ttu-id="2e789-141">Escolha **NSObject** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="2e789-141">Choose **NSObject** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="2e789-142">Abra **graphmanager. h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-142">Open **GraphManager.h** and replace its contents with the following code.</span></span>

    ```objc
    #import <Foundation/Foundation.h>
    #import <MSGraphClientSDK/MSGraphClientSDK.h>
    #import <MSGraphClientModels/MSGraphClientModels.h>
    #import <MSGraphClientModels/MSCollection.h>
    #import "AuthenticationManager.h"

    NS_ASSUME_NONNULL_BEGIN

    typedef void (^GetMeCompletionBlock)(MSGraphUser* _Nullable user, NSError* _Nullable error);

    @interface GraphManager : NSObject

    + (id) instance;
    - (void) getMeWithCompletionBlock: (GetMeCompletionBlock)completionBlock;

    @end

    NS_ASSUME_NONNULL_END
    ```

1. <span data-ttu-id="2e789-143">Abra **graphmanager. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-143">Open **GraphManager.m** and replace its contents with the following code.</span></span>

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

    - (void) getMeWithCompletionBlock:(GetMeCompletionBlock)completionBlock {
        // GET /me
        NSString* meUrlString = [NSString stringWithFormat:@"%@/me", MSGraphBaseURL];
        NSURL* meUrl = [[NSURL alloc] initWithString:meUrlString];
        NSMutableURLRequest* meRequest = [[NSMutableURLRequest alloc] initWithURL:meUrl];

        MSURLSessionDataTask* meDataTask =
        [[MSURLSessionDataTask alloc]
            initWithRequest:meRequest
            client:self.graphClient
            completion:^(NSData *data, NSURLResponse *response, NSError *error) {
                if (error) {
                    completionBlock(nil, error);
                    return;
                }

                // Deserialize the response as a user
                NSError* graphError;
                MSGraphUser* user = [[MSGraphUser alloc] initWithData:data error:&graphError];

                if (graphError) {
                    completionBlock(nil, graphError);
                } else {
                    completionBlock(user, nil);
                }
            }];

        // Execute the request
        [meDataTask execute];
    }

    @end
    ```

1. <span data-ttu-id="2e789-144">Abra **WelcomeViewController. m** e adicione as seguintes `#import` instruções na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="2e789-144">Open **WelcomeViewController.m** and add the following `#import` statements at the top of the file.</span></span>

    ```objc
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>
    ```

1. <span data-ttu-id="2e789-145">Adicione a seguinte propriedade à Declaração `WelcomeViewController` de interface.</span><span class="sxs-lookup"><span data-stu-id="2e789-145">Add the following property to the `WelcomeViewController` interface declaration.</span></span>

    ```objc
    @property SpinnerViewController* spinner;
    ```

1. <span data-ttu-id="2e789-146">Substitua o existente `viewDidLoad` pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2e789-146">Replace the existing `viewDidLoad` with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="ViewDidLoadSnippet":::

<span data-ttu-id="2e789-147">Se você salvar suas alterações e reiniciar o aplicativo agora, após entrar, a interface do usuário será atualizada com o nome de exibição e o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="2e789-147">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
