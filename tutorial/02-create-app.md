<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto Swift.

1. Abra o Xcode. No menu **arquivo** , selecione **novo**e, em seguida, **projeto**.
1. Escolha o modelo de **aplicativo exibir único** e selecione **Avançar**.

    ![Uma captura de tela da caixa de diálogo de seleção do modelo do Xcode](./images/xcode-select-template.png)

1. Defina o **nome** do produto `GraphTutorial` como e o **idioma** para **Objective-C**.
1. Preencha os campos restantes e selecione **Avançar**.
1. Escolha um local para o projeto e selecione **criar**.

## <a name="install-dependencies"></a>Instalar dependências

Antes de prosseguir, instale algumas dependências adicionais que serão usadas posteriormente.

- [Biblioteca de autenticação da Microsoft (MSAL) para IOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) para autenticação com o Azure AD.
- [SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.
- [SDK de modelos do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) para objetos fortemente tipados que representam os recursos do Microsoft Graph, como usuários ou eventos.

1. Encerre o Xcode.
1. Abra o terminal e altere o diretório para o local do seu projeto do **GraphTutorial** .
1. Execute o comando a seguir para criar um Podfile.

    ```Shell
    pod init
    ```

1. Abra o Podfile e adicione as seguintes linhas logo após a `use_frameworks!` linha.

    ```Ruby
    pod 'MSAL', '~> 1.1.1'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. Salve o Podfile e execute o seguinte comando para instalar as dependências.

    ```Shell
    pod install
    ```

1. Quando o comando for concluído, abra o **GraphTutorial. xcworkspace** recém-criado no Xcode.

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará os modos de exibição do aplicativo: uma página de entrada, um navegador de barra de guias, uma página de boas-vindas e uma página de calendário. Você também criará uma sobreposição de indicador de atividade.

### <a name="create-sign-in-page"></a>Criar página de entrada

1. Expanda a pasta **GraphTutorial** no Xcode e, em seguida, selecione o arquivo **ViewController. m** .
1. No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.m`.

    ![Uma captura de tela do Inspetor de arquivos](./images/rename-file.png)

1. Abra **SignInViewController. m** e substitua seu conteúdo pelo código a seguir.

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

1. Selecione o arquivo **ViewController. h** .
1. No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.h`.
1. Abra **SignInViewController. h** e altere todas as instâncias `ViewController` de `SignInViewController`para.

1. Abra o arquivo **Main. Storyboard** .
1. Expanda **Exibir controle cena**e selecione **Exibir controlador**.

    ![Uma captura de tela do Xcode com o controlador de modo de exibição selecionado](./images/storyboard-select-view-controller.png)

1. Selecione o **Inspetor de identidade**e, em seguida, altere o menu suspenso de **classe** para **SignInViewController**.

    ![Uma captura de tela do Inspetor de identidade](./images/change-class.png)

1. Selecione a **biblioteca**e arraste um **botão** para o **controlador de exibição de entrada**.

    ![Uma captura de tela da biblioteca no Xcode](./images/add-button-to-view.png)

1. Com o botão selecionado, selecione o **Inspetor de atributos** e altere o **título** do botão para `Sign In`.

    ![Uma captura de tela do campo título no Inspetor de atributos no Xcode](./images/set-button-title.png)

1. Com o botão selecionado, selecione o botão **alinhar** na parte inferior do storyboard. Selecione as duas opções **horizontalmente em contêiner** e **verticalmente em restrições de contêiner** , deixe seus valores como 0 e selecione **adicionar duas restrições**.

    ![Uma captura de tela das configurações de restrições de alinhamento no Xcode](./images/add-alignment-constraints.png)

1. Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.
1. Em **ações recebidas**, arraste o círculo não preenchido ao lado de **entrar** no botão. Selecione **retoque** no menu pop-up.

    ![Uma captura de tela de arrastar a ação de entrada para o botão no Xcode](./images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a>Criar barra de guias

1. Selecione a **biblioteca**e, em seguida, arraste um **controlador da barra de guias** para o storyboard.
1. Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.
1. Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador da barra de guias** no storyboard. Selecione **Mostrar modalmente** no menu pop-up.

    ![Uma captura de tela de arrastar um segue manual para o novo controlador de barra de guias no Xcode](./images/add-segue.png)

1. Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**. Definir o campo **identificador** como `userSignedIn`e definir a **apresentação** como **tela inteira**.

    ![Uma captura de tela do campo identificador no Inspetor de atributos no Xcode](./images/set-segue-identifier.png)

1. Selecione o **Item 1 cena**e, em seguida, selecione o **Inspetor de conexões**.
1. Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador de exibição de entrada** no storyboard. Selecione **Mostrar modalmente** no menu pop-up.
1. Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**. Definir o campo **identificador** como `userSignedOut`e definir a **apresentação** como **tela inteira**.

### <a name="create-welcome-page"></a>Criar página de boas-vindas

1. Selecione o arquivo **assets. xcassets** .
1. No menu **Editor** , selecione **Adicionar ativos**e, em seguida, **novo conjunto de imagens**.
1. Selecione o novo ativo de **imagem** e use o **Inspetor de atributos** para definir seu `DefaultUserPhoto` **nome** como.
1. Adicione qualquer imagem que você deseja que sirva como uma foto de perfil de usuário padrão.

    ![Uma captura de tela do modo de exibição ativo do conjunto de imagens no Xcode](./images/add-default-user-photo.png)

1. Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `WelcomeViewController`chamada. Escolha **UIViewController** na **subclasse de** Field.
1. Abra **WelcomeViewController. h** e adicione o código a seguir dentro `@interface` da declaração.

    ```objc
    @property (nonatomic) IBOutlet UIImageView *userProfilePhoto;
    @property (nonatomic) IBOutlet UILabel *userDisplayName;
    @property (nonatomic) IBOutlet UILabel *userEmail;
    ```

1. Abra **WelcomeViewController. m** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **Main. Storyboard**. Selecione a **cena item 1**e, em seguida, selecione o **Inspetor de identidade**. Altere o valor da **classe** para **WelcomeViewController**.
1. Usando a **biblioteca**, adicione os itens a seguir à **cena do item 1**.

    - Um **modo de exibição de imagem**
    - Dois **Rótulos**
    - Um **botão**

1. Usando o **Inspetor de conexões**, faça as seguintes conexões.

    - Vincule a saída **UserDisplayName** ao primeiro rótulo.
    - Vincule a saída **UserEmail** ao segundo rótulo.
    - Vincule a tomada **userProfilePhoto** à visualização de imagem.
    - Vincule a ação de **SignOut** recebida à **retoque**do botão dentro.

1. Selecione o modo de exibição imagem e, em seguida, selecione o **Inspetor de tamanho**.
1. Defina a **largura** e a **altura** como 196.
1. Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.
1. Use o botão **adicionar novas restrições** (ao lado do botão **alinhar** ) para adicionar as seguintes restrições:

    - Alinhar parte superior a: área segura, valor: 0
    - Espaço inferior para: nome de exibição do usuário, valor: Standard
    - Altura, valor: 196
    - Largura, valor: 196

    ![Uma captura de tela das novas configurações de restrições no Xcode](./images/add-new-constraints.png)

1. Selecione o primeiro rótulo e, em seguida, use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.
1. Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:

    - Espaço principal para: foto de perfil de usuário, valor: Standard
    - Espaço inferior para: email do usuário, valor: padrão

1. Selecione o segundo rótulo e, em seguida, selecione o **Inspetor de atributos**.
1. Altere a **cor** para **cor cinza escuro**e altere a **fonte** para **System 12,0**.
1. Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.
1. Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:

    - Espaço principal para: nome de exibição do usuário, valor: Standard
    - Espaço inferior para: sair, valor: 14

1. Selecione o botão e, em seguida, selecione o **Inspetor de atributos**.
1. Altere o **título** para `Sign Out`.
1. Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.
1. Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:

    - Espaço principal para: email do usuário, valor: 14

1. Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**. Altere o **título** para `Me`.
1. No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**

A cena de boas-vindas deve ser semelhante a esta quando você terminar.

![Uma captura de tela do layout da cena de boas-vindas](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a>Criar página de calendário

1. Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `CalendarViewController`chamada. Escolha **UIViewController** na **subclasse de** Field.
1. Abra **CalendarViewController. h** e adicione o código a seguir dentro `@interface` da declaração.

    ```objc
    @property (nonatomic) IBOutlet UITextView *calendarJSON;
    ```

1. Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.

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

1. Abra **Main. Storyboard**. Selecione a **cena item 2**e, em seguida, selecione o **Inspetor de identidade**. Altere o valor da **classe** para **CalendarViewController**.
1. Usando a **biblioteca**, adicione um **modo de exibição de texto** à **cena do item 2**.
1. Selecione o modo de exibição de texto que você acabou de adicionar. No **Editor**, escolha **incorporar no**e modo de **rolagem**.
1. Usando o **Inspetor de conexões**, conecte a tomada **calendarJSON** à visualização de texto.
1. Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**. Altere o **título** para `Calendar`.
1. No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**

A cena do calendário deve ser semelhante a esta quando você terminar.

![Uma captura de tela do layout de cena do calendário](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a>Criar indicador de atividade

1. Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `SpinnerViewController`chamada. Escolha **UIViewController** na **subclasse de** Field.
1. Abra **SpinnerViewController. h** e adicione o código a seguir dentro `@interface` da declaração.

    ```objc
    - (void) startWithContainer:(UIViewController*) container;
    - (void) stop;
    ```

1. Abra **SpinnerViewController. m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.m" id="SpinnerViewSnippet":::

## <a name="test-the-app"></a>O aplicativo de teste

Salve suas alterações e inicie o aplicativo. Você deve ser capaz de se mover entre as telas usando **Sign In** os botões entrar **e sair e** a barra de guias.

![Capturas de tela do aplicativo](./images/app-screens.png)
