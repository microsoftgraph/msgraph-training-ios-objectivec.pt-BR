<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Para fazer isso, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para IOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) no aplicativo.

1. Crie um novo arquivo de **lista de propriedades** no projeto do **GraphTutorial** chamado **AuthSettings. plist**.
1. Adicione os seguintes itens ao arquivo no dicionário **raiz** .

    | Chave | Tipo | Valor |
    |-----|------|-------|
    | `AppId` | String | A ID do aplicativo do portal do Azure |
    | `GraphScopes` | Matriz | Dois valores de cadeia `User.Read` de caracteres: e`Calendars.Read` |

    ![Uma captura de tela do arquivo AuthSettings. plist no Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **AuthSettings. plist** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

## <a name="implement-sign-in"></a>Implementar logon

Nesta seção, você irá configurar o projeto para o MSAL, criar uma classe do Gerenciador de autenticação e atualizar o aplicativo para entrar e sair.

### <a name="configure-project-for-msal"></a>Configurar o Project para o MSAL

1. Adicione um novo grupo de chaves aos recursos do seu projeto.
    1. Selecione o projeto **GraphTutorial** e, em seguida, **assinaturas & recursos**.
    1. Selecione **+ recurso**e, em seguida, clique duas vezes em **compartilhamento de chaves**.
    1. Adicione um grupo de chaves com o valor `com.microsoft.adalcache`.

1. Control clique **info. plist** e selecione **abrir como**e, em seguida, **código-fonte**.
1. Adicione o seguinte no `<dict>` elemento.

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

1. Abra **AppDelegate. m** e adicione a seguinte instrução de importação na parte superior do arquivo.

    ```objc
    #import <MSAL/MSAL.h>
    ```

1. Adicione a função a seguir à classe `AppDelegate`.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.m" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a>Criar o Gerenciador de autenticação

1. Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **AuthenticationManager**. Escolha **NSObject** na **subclasse de** Field.
1. Abra **AuthenticationManager. h** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.h" id="AuthManagerSnippet":::

1. Abra **AuthenticationManager. m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.m" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a>Adicionar entrada e saída

1. Abra o arquivo **SignInViewController. m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.m" id="SignInViewSnippet":::

1. Abra **WelcomeViewController. m** e adicione a seguinte `import` instrução à parte superior do arquivo.

    ```objc
    #import "AuthenticationManager.h"
    ```

1. Substitua a função `signOut` existente pelo seguinte.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="SignOutSnippet":::

1. Salve suas alterações e reinicie o aplicativo no simulador.

Se você entrar no aplicativo, verá um token de acesso exibido na janela de saída do Xcode.

![Uma captura de tela da janela de saída no Xcode mostrando um token de acesso](./images/access-token-output.png)

## <a name="get-user-details"></a>Obter detalhes do usuário

Nesta seção, você criará uma classe auxiliar para manter todas as chamadas para o Microsoft Graph e atualizará `WelcomeViewController` o para usar essa nova classe para obter o usuário conectado.

1. Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **graphmanager**. Escolha **NSObject** na **subclasse de** Field.
1. Abra **graphmanager. h** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **graphmanager. m** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **WelcomeViewController. m** e adicione as seguintes `#import` instruções na parte superior do arquivo.

    ```objc
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>
    ```

1. Adicione a seguinte propriedade à Declaração `WelcomeViewController` de interface.

    ```objc
    @property SpinnerViewController* spinner;
    ```

1. Substitua o existente `viewDidLoad` pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.m" id="ViewDidLoadSnippet":::

Se você salvar suas alterações e reiniciar o aplicativo agora, após entrar, a interface do usuário será atualizada com o nome de exibição e o endereço de email do usuário.
