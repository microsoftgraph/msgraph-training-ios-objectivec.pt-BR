<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto Objective-C.

1. Abra o Xcode. No menu **Arquivo,** selecione **Novo** e **Projeto.**
1. Escolha o **modelo de** aplicativo e selecione **Próximo**.

    ![Uma captura de tela da caixa de diálogo de seleção do modelo Xcode](./images/xcode-select-template.png)

1. Definir o **nome do** produto e `GraphTutorial` o **idioma** como **Objective-C.**
1. Preencha os campos restantes e selecione **Next**.
1. Escolha um local para o projeto e selecione **Criar**.

## <a name="install-dependencies"></a>Instalar dependências

Antes de continuar, instale algumas dependências adicionais que você usará mais tarde.

- [Biblioteca de Autenticação da Microsoft (MSAL) para iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) para autenticação no Azure AD.
- [SDK do Microsoft Graph para Objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas ao Microsoft Graph.
- [SDK de Modelos do Microsoft Graph para Objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) para objetos fortemente digitados que representam recursos do Microsoft Graph, como usuários ou eventos.

1. Saia do Xcode.
1. Abra o Terminal e altere o diretório para o local do **seu projeto GraphTutorial.**
1. Execute o seguinte comando para criar um Podfile.

    ```Shell
    pod init
    ```

1. Abra o Podfile e adicione as linhas a seguir logo após a `use_frameworks!` linha.

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. Salve o Podfile e execute o seguinte comando para instalar as dependências.

    ```Shell
    pod install
    ```

1. Depois que o comando terminar, abra o **GraphTutorial.xcworkspace recém-criado** no Xcode.

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará as exibições para o aplicativo: uma página de login, um navegador da barra de guias, uma página de boas-vindas e uma página de calendário. Você também criará uma sobreposição do indicador de atividade.

### <a name="create-sign-in-page"></a>Criar página de login

1. Expanda **a pasta GraphTutorial** no Xcode e selecione o **arquivo ViewController.m.**
1. No **Inspetor de Arquivos,** altere **o nome** do arquivo para `SignInViewController.m` .

    ![Uma captura de tela do Inspetor de Arquivos](./images/rename-file.png)

1. Abra **SignInViewController.m e** substitua seu conteúdo pelo código a seguir.

    ```objc
    #import "SignInViewController.h"

    @interface SignInViewController ()

    @end

    @implementation SignInViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.
    }

    - (IBAction)signIn {
        [self performSegueWithIdentifier: @"userSignedIn" sender: nil];
    }
    @end
    ```

1. Selecione o **arquivo ViewController.h.**
1. No **Inspetor de Arquivos,** altere **o nome** do arquivo para `SignInViewController.h` .
1. Abra **SignInViewController.h** e altere todas as instâncias `ViewController` de para `SignInViewController` .

1. Abra o **arquivo Main.storyboard.**
1. Expanda **a cena do controlador** de exibição e selecione Controlador de **exibição.**

    ![Captura de tela do Xcode com o Controlador de Exibição selecionado](./images/storyboard-select-view-controller.png)

1. Selecione o **Inspetor de Identidade,** em seguida, altere **o** menu suspenso de classe para **SignInViewController**.

    ![Uma captura de tela do Inspetor de Identidade](./images/change-class.png)

1. Selecione a **biblioteca** e arraste um **botão para o** controlador de **exibição entrar.**

    ![Uma captura de tela da Biblioteca no Xcode](./images/add-button-to-view.png)

1. Com o botão selecionado, selecione **o Inspetor de Atributos** e altere o **Título** do botão para `Sign In` .

    ![Uma captura de tela do campo Título no Inspetor de Atributos no Xcode](./images/set-button-title.png)

1. Com o botão selecionado, selecione **o botão Alinhar** na parte inferior do storyboard. Selecione **horizontalmente no contêiner** e **verticalmente** em restrições de contêiner, deixe seus valores como 0 e, em seguida, selecione **Adicionar 2 restrições**.

    ![Uma captura de tela das configurações de restrições de alinhamento no Xcode](./images/add-alignment-constraints.png)

1. Selecione o **Controlador de Exibição de Logo in,** em seguida, selecione o **Inspetor de Conexões.**
1. Em **Ações Recebidas,** arraste o círculo não preenchido ao lado para **entrar** no botão. Selecione **Tocar para Dentro** no menu pop-up.

    ![Uma captura de tela arrastando a ação de entrar para o botão no Xcode](./images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a>Criar barra de guias

1. Selecione a **Biblioteca** e arraste um Controlador de **Barra de Guias** para o storyboard.
1. Selecione o **Controlador de Exibição de Logo in,** em seguida, selecione o **Inspetor de Conexões.**
1. Em **Segues Disparados,** arraste o círculo não preenchido ao lado de **manual** para o Controlador da Barra **de Guias** no storyboard. Selecione **Apresentar Modalmente** no menu pop-up.

    ![Uma captura de tela de arrastar um segue manual para o novo Controlador de Barra de Guias no Xcode](./images/add-segue.png)

1. Selecione o segue que você acabou de adicionar e selecione o **Inspetor de Atributos.** Definir o **campo Identificador** como `userSignedIn` e definir **Apresentação** como **Tela Inteira.**

    ![Captura de tela do campo Identificador no Inspetor de Atributos no Xcode](./images/set-segue-identifier.png)

1. Selecione a **cena item 1** e, em seguida, selecione o **Inspetor de conexões**.
1. Em **Segues Disparados,** arraste o círculo não preenchido ao lado de **manual** para o Controlador **de Modo** de Exibição entrar no storyboard. Selecione **Apresentar Modalmente** no menu pop-up.
1. Selecione o segue que você acabou de adicionar e selecione o **Inspetor de Atributos.** Definir o **campo Identificador** como `userSignedOut` e definir **Apresentação** como **Tela Inteira.**

### <a name="create-welcome-page"></a>Criar página de boas-vindas

1. Selecione o **arquivo Assets.xcassets.**
1. No **menu Editor,** selecione **Adicionar Novo Ativo** e, em seguida, Conjunto de **Imagens.**
1. Selecione o novo **ativo de** imagem e use o Inspetor **de Atributos** para definir **seu nome** como `DefaultUserPhoto` .
1. Adicione qualquer imagem que você gosta de servir como uma foto de perfil de usuário padrão.

    ![Uma captura de tela da exibição de ativo conjunto de imagens no Xcode](./images/add-default-user-photo.png)

1. Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `WelcomeViewController` . Escolha **UIViewController** na **Subclasse do** campo.
1. Abra **WelcomeViewController.h** e adicione o seguinte código à `@interface` declaração.

    ```objc
    @property (nonatomic) IBOutlet UIImageView *userProfilePhoto;
    @property (nonatomic) IBOutlet UILabel *userDisplayName;
    @property (nonatomic) IBOutlet UILabel *userEmail;
    ```

1. Abra **WelcomeViewController.m** e substitua seu conteúdo pelo código a seguir.

    ```objc
    #import "WelcomeViewController.h"

    @interface WelcomeViewController ()

    @end

    @implementation WelcomeViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.

        // TEMPORARY
        self.userProfilePhoto.image = [UIImage imageNamed:@"DefaultUserPhoto"];
        self.userDisplayName.text = @"Default User";
        [self.userDisplayName sizeToFit];
        self.userEmail.text = @"default@contoso.com";
        [self.userEmail sizeToFit];
    }

    - (IBAction)signOut {
        [self performSegueWithIdentifier: @"userSignedOut" sender: nil];
    }

    @end
    ```

1. Abra **Main.storyboard**. Selecione a **Cena do Item 1** e, em seguida, selecione o **Inspetor de Identidade.** Altere **o valor** de classe para **WelcomeViewController**.
1. Usando a **biblioteca**, adicione os seguintes itens à cena **item 1**.

    - Um **exibição de imagem**
    - Dois **Rótulos**
    - Um **botão**

1. Usando o **Inspetor de Conexões,** faça as seguintes conexões.

    - **Vincule a saída userDisplayName** ao primeiro rótulo.
    - **Vincule a saída userEmail** ao segundo rótulo.
    - Vincule **a saída userProfilePhoto** à exibição de imagem.
    - **Vincule a ação** de logoff recebida ao botão Touch Up **Inside**.

1. Selecione o visualizador de imagem e, em seguida, selecione o **Inspetor de tamanho.**
1. De definida **a largura** e **a altura** como 196.
1. Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.
1. Use o **botão Adicionar Novas Restrições** (ao lado do botão **Alinhar)** para adicionar as seguintes restrições:

    - Alinhar de cima para: Área de Segurança, valor: 0
    - Espaço Inferior para: Nome de Exibição do Usuário, valor: Padrão
    - Altura, valor: 196
    - Largura, valor: 196

    ![Captura de tela das novas configurações de restrições no Xcode](./images/add-new-constraints.png)

1. Selecione o primeiro rótulo e, em seguida, use o botão **Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor 0.
1. Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:

    - Espaço Superior para: Foto do Perfil de Usuário, valor: Padrão
    - Espaço Inferior para: Email do Usuário, valor: Padrão

1. Selecione o segundo rótulo e selecione o **Inspetor de Atributos.**
1. Altere **a cor** para cinza escuro **e** altere a **fonte** para o **sistema 12.0**.
1. Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.
1. Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:

    - Espaço Superior para: Nome de Exibição do Usuário, valor: Padrão
    - Espaço Inferior para: Sair, valor: 14

1. Selecione o botão e selecione o **Inspetor de Atributos.**
1. Altere **o título** para `Sign Out` .
1. Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.
1. Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:

    - Espaço Superior para: Email do Usuário, valor: 14

1. Selecione o item da barra de guias na parte inferior da cena e selecione o **Inspetor de Atributos.** Altere **o título** para `Me` .
1. No **menu Editor,** selecione Resolver Problemas **de Layout** Automático e, em seguida, selecione Adicionar Restrições **Ausentes** abaixo de Todos os Exibições no Controlador de Modo de **Exibição de Boas-vindas.**

A cena de boas-vindas deve ser semelhante a esta quando você terminar.

![Uma captura de tela do layout da cena de boas-vindas](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a>Criar página de calendário

1. Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `CalendarViewController` . Escolha **UIViewController** na **Subclasse do** campo.
1. Abra **CalendarViewController.h** e adicione o seguinte código à `@interface` declaração.

    ```objc
    @property (nonatomic) IBOutlet UITextView *calendarJSON;
    ```

1. Abra **CalendarViewController.m e** substitua seu conteúdo pelo código a seguir.

    ```objc
    #import "CalendarViewController.h"

    @interface CalendarViewController ()

    @end

    @implementation CalendarViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.

        // TEMPORARY
        self.calendarJSON.text = @"Calendar";
        [self.calendarJSON sizeToFit];
    }

    @end
    ```

1. Abra **Main.storyboard**. Selecione a **Cena do Item 2** e, em seguida, selecione o **Inspetor de Identidade.** Altere **o valor** de classe para **CalendarViewController**.
1. Usando a **biblioteca**, adicione um **exibição de texto** à cena item **2**.
1. Selecione o exibição de texto que você acabou de adicionar. On the **Editor**, choose **Embed In**, then **Scroll View**.
1. Usando o **Inspetor de Conexões,** conecte **a saída calendarJSON** ao exibição de texto.
1. Selecione o item da barra de guias na parte inferior da cena e selecione o **Inspetor de Atributos.** Altere **o título** para `Calendar` .
1. No **menu Editor,** selecione Resolver Problemas **de Layout** Automático e, em seguida, selecione Adicionar Restrições **Ausentes** abaixo de Todos os Exibições no Controlador de Modo de **Exibição de Boas-vindas.**

A cena do calendário deve ser semelhante a esta quando você terminar.

![Uma captura de tela do layout da cena calendário](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a>Criar indicador de atividade

1. Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `SpinnerViewController` . Escolha **UIViewController** na **Subclasse do** campo.
1. Abra **SpinnerViewController.h** e adicione o seguinte código à `@interface` declaração.

    ```objc
    - (void) startWithContainer:(UIViewController*) container;
    - (void) stop;
    ```

1. Abra **SpinnerViewController.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.m" id="SpinnerViewSnippet":::

## <a name="test-the-app"></a>O aplicativo de teste

Salve suas alterações e iniciar o aplicativo. Você deve ser capaz de se  mover entre as telas usando os botões Entrar e **Sair** e a barra de guias.

![Capturas de tela do aplicativo](./images/app-screens.png)
