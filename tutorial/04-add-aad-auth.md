<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Para fazer isso, você integrará a Biblioteca de Autenticação da [Microsoft (MSAL) para iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) ao aplicativo.

1. Crie um novo **arquivo de lista de** propriedades no projeto **GraphTutorial** chamado **AuthSettings.plist**.
1. Adicione os itens a seguir ao arquivo no **dicionário** raiz.

    | Chave | Tipo | Valor |
    |-----|------|-------|
    | `AppId` | Cadeia de caracteres | A ID do aplicativo do portal do Azure |
    | `GraphScopes` | Matriz | Três valores de cadeia de `User.Read` caracteres: `MailboxSettings.Read` , e `Calendars.ReadWrite` |

    ![Captura de tela do arquivo AuthSettings.plist no Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo **AuthSettings.plist** do controle de código-fonte para evitar o vazamento inadvertida da ID do aplicativo.

## <a name="implement-sign-in"></a>Implementar a login

Nesta seção, você configurará o projeto para MSAL, criará uma classe de gerenciador de autenticação e atualizará o aplicativo para entrar e sair.

### <a name="configure-project-for-msal"></a>Configurar o projeto para MSAL

1. Adicione um novo grupo de chaves aos recursos do seu projeto.
    1. Selecione o **projeto GraphTutorial** e, em seguida, **assinando & recursos.**
    1. Select **+ Capability**, then double-click **Keychain Sharing**.
    1. Adicione um grupo de chaves com o `com.microsoft.adalcache` valor.

1. Controle clique **em Info.plist** e selecione **Abrir como**, em seguida, **Código-fonte**.
1. Adicione o seguinte dentro do `<dict>` elemento.

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

1. Abra **AppDelegate.m** e adicione a seguinte instrução de importação na parte superior do arquivo.

    ```objc
    #import <MSAL/MSAL.h>
    ```

1. Adicione a função a seguir à classe `AppDelegate`.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.m" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a>Criar gerenciador de autenticação

1. Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **AuthenticationManager**. Escolha **NSObject** na **Subclasse do** campo.
1. Abra **AuthenticationManager.h** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.h" id="AuthManagerSnippet":::

1. Abra **AuthenticationManager.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.m" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a>Adicionar entrar e sair

1. Abra o **arquivo SignInViewController.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.m" id="SignInViewSnippet":::

1. Abra **WelcomeViewController.m** e adicione a `import` instrução a seguir na parte superior do arquivo.

    ```objc
    #import "AuthenticationManager.h"
    ```

1. Substitua a função `signOut` existente pelo seguinte.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="SignOutSnippet":::

1. Salve suas alterações e reinicie o aplicativo no Simulador.

Se você entrar no aplicativo, deverá ver um token de acesso exibido na janela de saída no Xcode.

![Uma captura de tela da janela de saída no Xcode mostrando um token de acesso](./images/access-token-output.png)

## <a name="get-user-details"></a>Obter detalhes do usuário

Nesta seção, você criará uma classe auxiliar para manter todas as chamadas para o Microsoft Graph e atualizar para usar essa nova classe para obter o usuário `WelcomeViewController` conectado.

1. Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **GraphManager**. Escolha **NSObject** na **Subclasse do** campo.
1. Abra **GraphManager.h** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **GraphManager.m** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **WelcomeViewController.m** e adicione as instruções a `#import` seguir na parte superior do arquivo.

    ```objc
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>
    ```

1. Adicione a propriedade a seguir à declaração `WelcomeViewController` da interface.

    ```objc
    @property SpinnerViewController* spinner;
    ```

1. Substitua o existente `viewDidLoad` pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="ViewDidLoadSnippet":::

Se você salvar suas alterações e reiniciar o aplicativo agora, depois de entrar na interface do usuário for atualizado com o nome de exibição e o endereço de email do usuário.
