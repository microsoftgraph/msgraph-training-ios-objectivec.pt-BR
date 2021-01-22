<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="053ba-101">Comece criando um novo projeto Objective-C.</span><span class="sxs-lookup"><span data-stu-id="053ba-101">Begin by creating a new Objective-C project.</span></span>

1. <span data-ttu-id="053ba-102">Abra o Xcode.</span><span class="sxs-lookup"><span data-stu-id="053ba-102">Open Xcode.</span></span> <span data-ttu-id="053ba-103">No menu **Arquivo,** selecione **Novo** e **Projeto.**</span><span class="sxs-lookup"><span data-stu-id="053ba-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="053ba-104">Escolha o **modelo de** aplicativo e selecione **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="053ba-104">Choose the **App** template and select **Next**.</span></span>

    ![Uma captura de tela da caixa de diálogo de seleção do modelo Xcode](./images/xcode-select-template.png)

1. <span data-ttu-id="053ba-106">Definir o **nome do** produto e `GraphTutorial` o **idioma** como **Objective-C.**</span><span class="sxs-lookup"><span data-stu-id="053ba-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Objective-C**.</span></span>
1. <span data-ttu-id="053ba-107">Preencha os campos restantes e selecione **Next**.</span><span class="sxs-lookup"><span data-stu-id="053ba-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="053ba-108">Escolha um local para o projeto e selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="053ba-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="053ba-109">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="053ba-109">Install dependencies</span></span>

<span data-ttu-id="053ba-110">Antes de continuar, instale algumas dependências adicionais que você usará mais tarde.</span><span class="sxs-lookup"><span data-stu-id="053ba-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="053ba-111">[Biblioteca de Autenticação da Microsoft (MSAL) para iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) para autenticação no Azure AD.</span><span class="sxs-lookup"><span data-stu-id="053ba-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="053ba-112">[SDK do Microsoft Graph para Objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas ao Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="053ba-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="053ba-113">[SDK de Modelos do Microsoft Graph para Objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) para objetos fortemente digitados que representam recursos do Microsoft Graph, como usuários ou eventos.</span><span class="sxs-lookup"><span data-stu-id="053ba-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="053ba-114">Saia do Xcode.</span><span class="sxs-lookup"><span data-stu-id="053ba-114">Quit Xcode.</span></span>
1. <span data-ttu-id="053ba-115">Abra o Terminal e altere o diretório para o local do **seu projeto GraphTutorial.**</span><span class="sxs-lookup"><span data-stu-id="053ba-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="053ba-116">Execute o seguinte comando para criar um Podfile.</span><span class="sxs-lookup"><span data-stu-id="053ba-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="053ba-117">Abra o Podfile e adicione as linhas a seguir logo após a `use_frameworks!` linha.</span><span class="sxs-lookup"><span data-stu-id="053ba-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="053ba-118">Salve o Podfile e execute o seguinte comando para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="053ba-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="053ba-119">Depois que o comando terminar, abra o **GraphTutorial.xcworkspace recém-criado** no Xcode.</span><span class="sxs-lookup"><span data-stu-id="053ba-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="053ba-120">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="053ba-120">Design the app</span></span>

<span data-ttu-id="053ba-121">Nesta seção, você criará as exibições para o aplicativo: uma página de login, um navegador da barra de guias, uma página de boas-vindas e uma página de calendário.</span><span class="sxs-lookup"><span data-stu-id="053ba-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="053ba-122">Você também criará uma sobreposição do indicador de atividade.</span><span class="sxs-lookup"><span data-stu-id="053ba-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="053ba-123">Criar página de login</span><span class="sxs-lookup"><span data-stu-id="053ba-123">Create sign in page</span></span>

1. <span data-ttu-id="053ba-124">Expanda **a pasta GraphTutorial** no Xcode e selecione o **arquivo ViewController.m.**</span><span class="sxs-lookup"><span data-stu-id="053ba-124">Expand the **GraphTutorial** folder in Xcode, then select the **ViewController.m** file.</span></span>
1. <span data-ttu-id="053ba-125">No **Inspetor de Arquivos,** altere **o nome** do arquivo para `SignInViewController.m` .</span><span class="sxs-lookup"><span data-stu-id="053ba-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.m`.</span></span>

    ![Uma captura de tela do Inspetor de Arquivos](./images/rename-file.png)

1. <span data-ttu-id="053ba-127">Abra **SignInViewController.m e** substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="053ba-127">Open **SignInViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="053ba-128">Selecione o **arquivo ViewController.h.**</span><span class="sxs-lookup"><span data-stu-id="053ba-128">Select the **ViewController.h** file.</span></span>
1. <span data-ttu-id="053ba-129">No **Inspetor de Arquivos,** altere **o nome** do arquivo para `SignInViewController.h` .</span><span class="sxs-lookup"><span data-stu-id="053ba-129">In the **File Inspector**, change the **Name** of the file to `SignInViewController.h`.</span></span>
1. <span data-ttu-id="053ba-130">Abra **SignInViewController.h** e altere todas as instâncias `ViewController` de para `SignInViewController` .</span><span class="sxs-lookup"><span data-stu-id="053ba-130">Open **SignInViewController.h** and change all instances of `ViewController` to `SignInViewController`.</span></span>

1. <span data-ttu-id="053ba-131">Abra o **arquivo Main.storyboard.**</span><span class="sxs-lookup"><span data-stu-id="053ba-131">Open the **Main.storyboard** file.</span></span>
1. <span data-ttu-id="053ba-132">Expanda **a cena do controlador** de exibição e selecione Controlador de **exibição.**</span><span class="sxs-lookup"><span data-stu-id="053ba-132">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Captura de tela do Xcode com o Controlador de Exibição selecionado](./images/storyboard-select-view-controller.png)

1. <span data-ttu-id="053ba-134">Selecione o **Inspetor de Identidade,** em seguida, altere **o** menu suspenso de classe para **SignInViewController**.</span><span class="sxs-lookup"><span data-stu-id="053ba-134">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Uma captura de tela do Inspetor de Identidade](./images/change-class.png)

1. <span data-ttu-id="053ba-136">Selecione a **biblioteca** e arraste um **botão para o** controlador de **exibição entrar.**</span><span class="sxs-lookup"><span data-stu-id="053ba-136">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Uma captura de tela da Biblioteca no Xcode](./images/add-button-to-view.png)

1. <span data-ttu-id="053ba-138">Com o botão selecionado, selecione **o Inspetor de Atributos** e altere o **Título** do botão para `Sign In` .</span><span class="sxs-lookup"><span data-stu-id="053ba-138">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Uma captura de tela do campo Título no Inspetor de Atributos no Xcode](./images/set-button-title.png)

1. <span data-ttu-id="053ba-140">Com o botão selecionado, selecione **o botão Alinhar** na parte inferior do storyboard.</span><span class="sxs-lookup"><span data-stu-id="053ba-140">With the button selected, select the **Align** button at the bottom of the storyboard.</span></span> <span data-ttu-id="053ba-141">Selecione **horizontalmente no contêiner** e **verticalmente** em restrições de contêiner, deixe seus valores como 0 e, em seguida, selecione **Adicionar 2 restrições**.</span><span class="sxs-lookup"><span data-stu-id="053ba-141">Select both the **Horizontally in container** and **Vertically in container** constraints, leave their values as 0, then select **Add 2 constraints**.</span></span>

    ![Uma captura de tela das configurações de restrições de alinhamento no Xcode](./images/add-alignment-constraints.png)

1. <span data-ttu-id="053ba-143">Selecione o **Controlador de Exibição de Logo in,** em seguida, selecione o **Inspetor de Conexões.**</span><span class="sxs-lookup"><span data-stu-id="053ba-143">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="053ba-144">Em **Ações Recebidas,** arraste o círculo não preenchido ao lado para **entrar** no botão.</span><span class="sxs-lookup"><span data-stu-id="053ba-144">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="053ba-145">Selecione **Tocar para Dentro** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="053ba-145">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Uma captura de tela arrastando a ação de entrar para o botão no Xcode](./images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a><span data-ttu-id="053ba-147">Criar barra de guias</span><span class="sxs-lookup"><span data-stu-id="053ba-147">Create tab bar</span></span>

1. <span data-ttu-id="053ba-148">Selecione a **Biblioteca** e arraste um Controlador de **Barra de Guias** para o storyboard.</span><span class="sxs-lookup"><span data-stu-id="053ba-148">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="053ba-149">Selecione o **Controlador de Exibição de Logo in,** em seguida, selecione o **Inspetor de Conexões.**</span><span class="sxs-lookup"><span data-stu-id="053ba-149">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="053ba-150">Em **Segues Disparados,** arraste o círculo não preenchido ao lado de **manual** para o Controlador da Barra **de Guias** no storyboard.</span><span class="sxs-lookup"><span data-stu-id="053ba-150">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="053ba-151">Selecione **Apresentar Modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="053ba-151">Select **Present Modally** in the pop-up menu.</span></span>

    ![Uma captura de tela de arrastar um segue manual para o novo Controlador de Barra de Guias no Xcode](./images/add-segue.png)

1. <span data-ttu-id="053ba-153">Selecione o segue que você acabou de adicionar e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-153">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="053ba-154">Definir o **campo Identificador** como `userSignedIn` e definir **Apresentação** como **Tela Inteira.**</span><span class="sxs-lookup"><span data-stu-id="053ba-154">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Captura de tela do campo Identificador no Inspetor de Atributos no Xcode](./images/set-segue-identifier.png)

1. <span data-ttu-id="053ba-156">Selecione a **cena item 1** e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="053ba-156">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="053ba-157">Em **Segues Disparados,** arraste o círculo não preenchido ao lado de **manual** para o Controlador **de Modo** de Exibição entrar no storyboard.</span><span class="sxs-lookup"><span data-stu-id="053ba-157">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="053ba-158">Selecione **Apresentar Modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="053ba-158">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="053ba-159">Selecione o segue que você acabou de adicionar e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-159">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="053ba-160">Definir o **campo Identificador** como `userSignedOut` e definir **Apresentação** como **Tela Inteira.**</span><span class="sxs-lookup"><span data-stu-id="053ba-160">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="053ba-161">Criar página de boas-vindas</span><span class="sxs-lookup"><span data-stu-id="053ba-161">Create welcome page</span></span>

1. <span data-ttu-id="053ba-162">Selecione o **arquivo Assets.xcassets.**</span><span class="sxs-lookup"><span data-stu-id="053ba-162">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="053ba-163">No **menu Editor,** selecione **Adicionar Novo Ativo** e, em seguida, Conjunto de **Imagens.**</span><span class="sxs-lookup"><span data-stu-id="053ba-163">On the **Editor** menu, select **Add New Asset**, then **Image Set**.</span></span>
1. <span data-ttu-id="053ba-164">Selecione o novo **ativo de** imagem e use o Inspetor **de Atributos** para definir **seu nome** como `DefaultUserPhoto` .</span><span class="sxs-lookup"><span data-stu-id="053ba-164">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="053ba-165">Adicione qualquer imagem que você gosta de servir como uma foto de perfil de usuário padrão.</span><span class="sxs-lookup"><span data-stu-id="053ba-165">Add any image you like to serve as a default user profile photo.</span></span>

    ![Uma captura de tela da exibição de ativo conjunto de imagens no Xcode](./images/add-default-user-photo.png)

1. <span data-ttu-id="053ba-167">Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `WelcomeViewController` .</span><span class="sxs-lookup"><span data-stu-id="053ba-167">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="053ba-168">Escolha **UIViewController** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="053ba-168">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="053ba-169">Abra **WelcomeViewController.h** e adicione o seguinte código à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="053ba-169">Open **WelcomeViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UIImageView *userProfilePhoto;
    @property (nonatomic) IBOutlet UILabel *userDisplayName;
    @property (nonatomic) IBOutlet UILabel *userEmail;
    ```

1. <span data-ttu-id="053ba-170">Abra **WelcomeViewController.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="053ba-170">Open **WelcomeViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="053ba-171">Abra **Main.storyboard**.</span><span class="sxs-lookup"><span data-stu-id="053ba-171">Open **Main.storyboard**.</span></span> <span data-ttu-id="053ba-172">Selecione a **Cena do Item 1** e, em seguida, selecione o **Inspetor de Identidade.**</span><span class="sxs-lookup"><span data-stu-id="053ba-172">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="053ba-173">Altere **o valor** de classe para **WelcomeViewController**.</span><span class="sxs-lookup"><span data-stu-id="053ba-173">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="053ba-174">Usando a **biblioteca**, adicione os seguintes itens à cena **item 1**.</span><span class="sxs-lookup"><span data-stu-id="053ba-174">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="053ba-175">Um **exibição de imagem**</span><span class="sxs-lookup"><span data-stu-id="053ba-175">One **Image View**</span></span>
    - <span data-ttu-id="053ba-176">Dois **Rótulos**</span><span class="sxs-lookup"><span data-stu-id="053ba-176">Two **Labels**</span></span>
    - <span data-ttu-id="053ba-177">Um **botão**</span><span class="sxs-lookup"><span data-stu-id="053ba-177">One **Button**</span></span>

1. <span data-ttu-id="053ba-178">Usando o **Inspetor de Conexões,** faça as seguintes conexões.</span><span class="sxs-lookup"><span data-stu-id="053ba-178">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="053ba-179">**Vincule a saída userDisplayName** ao primeiro rótulo.</span><span class="sxs-lookup"><span data-stu-id="053ba-179">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="053ba-180">**Vincule a saída userEmail** ao segundo rótulo.</span><span class="sxs-lookup"><span data-stu-id="053ba-180">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="053ba-181">Vincule **a saída userProfilePhoto** à exibição de imagem.</span><span class="sxs-lookup"><span data-stu-id="053ba-181">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="053ba-182">**Vincule a ação** de logoff recebida ao botão Touch Up **Inside**.</span><span class="sxs-lookup"><span data-stu-id="053ba-182">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="053ba-183">Selecione o visualizador de imagem e, em seguida, selecione o **Inspetor de tamanho.**</span><span class="sxs-lookup"><span data-stu-id="053ba-183">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="053ba-184">De definida **a largura** e **a altura** como 196.</span><span class="sxs-lookup"><span data-stu-id="053ba-184">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="053ba-185">Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="053ba-185">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="053ba-186">Use o **botão Adicionar Novas Restrições** (ao lado do botão **Alinhar)** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="053ba-186">Use the **Add New Constraints** button (next to the **Align** button) to add the following constraints:</span></span>

    - <span data-ttu-id="053ba-187">Alinhar de cima para: Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="053ba-187">Align Top to: Safe Area, value: 0</span></span>
    - <span data-ttu-id="053ba-188">Espaço Inferior para: Nome de Exibição do Usuário, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="053ba-188">Bottom Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="053ba-189">Altura, valor: 196</span><span class="sxs-lookup"><span data-stu-id="053ba-189">Height, value: 196</span></span>
    - <span data-ttu-id="053ba-190">Largura, valor: 196</span><span class="sxs-lookup"><span data-stu-id="053ba-190">Width, value: 196</span></span>

    ![Captura de tela das novas configurações de restrições no Xcode](./images/add-new-constraints.png)

1. <span data-ttu-id="053ba-192">Selecione o primeiro rótulo e, em seguida, use o botão **Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor 0.</span><span class="sxs-lookup"><span data-stu-id="053ba-192">Select the first label, then use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="053ba-193">Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="053ba-193">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="053ba-194">Espaço Superior para: Foto do Perfil de Usuário, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="053ba-194">Top Space to: User Profile Photo, value: Standard</span></span>
    - <span data-ttu-id="053ba-195">Espaço Inferior para: Email do Usuário, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="053ba-195">Bottom Space to: User Email, value: Standard</span></span>

1. <span data-ttu-id="053ba-196">Selecione o segundo rótulo e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-196">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="053ba-197">Altere **a cor** para cinza escuro **e** altere a **fonte** para o **sistema 12.0**.</span><span class="sxs-lookup"><span data-stu-id="053ba-197">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="053ba-198">Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="053ba-198">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="053ba-199">Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="053ba-199">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="053ba-200">Espaço Superior para: Nome de Exibição do Usuário, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="053ba-200">Top Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="053ba-201">Espaço Inferior para: Sair, valor: 14</span><span class="sxs-lookup"><span data-stu-id="053ba-201">Bottom Space to: Sign Out, value: 14</span></span>

1. <span data-ttu-id="053ba-202">Selecione o botão e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-202">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="053ba-203">Altere **o título** para `Sign Out` .</span><span class="sxs-lookup"><span data-stu-id="053ba-203">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="053ba-204">Use o **botão Alinhar** para adicionar **a restrição Horizontalmente no** contêiner com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="053ba-204">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="053ba-205">Use o **botão Adicionar Novas Restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="053ba-205">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="053ba-206">Espaço Superior para: Email do Usuário, valor: 14</span><span class="sxs-lookup"><span data-stu-id="053ba-206">Top Space to: User Email, value: 14</span></span>

1. <span data-ttu-id="053ba-207">Selecione o item da barra de guias na parte inferior da cena e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-207">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="053ba-208">Altere **o título** para `Me` .</span><span class="sxs-lookup"><span data-stu-id="053ba-208">Change the **Title** to `Me`.</span></span>
1. <span data-ttu-id="053ba-209">No **menu Editor,** selecione Resolver Problemas **de Layout** Automático e, em seguida, selecione Adicionar Restrições **Ausentes** abaixo de Todos os Exibições no Controlador de Modo de **Exibição de Boas-vindas.**</span><span class="sxs-lookup"><span data-stu-id="053ba-209">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="053ba-210">A cena de boas-vindas deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="053ba-210">The welcome scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout da cena de boas-vindas](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="053ba-212">Criar página de calendário</span><span class="sxs-lookup"><span data-stu-id="053ba-212">Create calendar page</span></span>

1. <span data-ttu-id="053ba-213">Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `CalendarViewController` .</span><span class="sxs-lookup"><span data-stu-id="053ba-213">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="053ba-214">Escolha **UIViewController** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="053ba-214">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="053ba-215">Abra **CalendarViewController.h** e adicione o seguinte código à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="053ba-215">Open **CalendarViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UITextView *calendarJSON;
    ```

1. <span data-ttu-id="053ba-216">Abra **CalendarViewController.m e** substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="053ba-216">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="053ba-217">Abra **Main.storyboard**.</span><span class="sxs-lookup"><span data-stu-id="053ba-217">Open **Main.storyboard**.</span></span> <span data-ttu-id="053ba-218">Selecione a **Cena do Item 2** e, em seguida, selecione o **Inspetor de Identidade.**</span><span class="sxs-lookup"><span data-stu-id="053ba-218">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="053ba-219">Altere **o valor** de classe para **CalendarViewController**.</span><span class="sxs-lookup"><span data-stu-id="053ba-219">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="053ba-220">Usando a **biblioteca**, adicione um **exibição de texto** à cena item **2**.</span><span class="sxs-lookup"><span data-stu-id="053ba-220">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="053ba-221">Selecione o exibição de texto que você acabou de adicionar.</span><span class="sxs-lookup"><span data-stu-id="053ba-221">Select the text view you just added.</span></span> <span data-ttu-id="053ba-222">On the **Editor**, choose **Embed In**, then **Scroll View**.</span><span class="sxs-lookup"><span data-stu-id="053ba-222">On the **Editor**, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="053ba-223">Usando o **Inspetor de Conexões,** conecte **a saída calendarJSON** ao exibição de texto.</span><span class="sxs-lookup"><span data-stu-id="053ba-223">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. <span data-ttu-id="053ba-224">Selecione o item da barra de guias na parte inferior da cena e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="053ba-224">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="053ba-225">Altere **o título** para `Calendar` .</span><span class="sxs-lookup"><span data-stu-id="053ba-225">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="053ba-226">No **menu Editor,** selecione Resolver Problemas **de Layout** Automático e, em seguida, selecione Adicionar Restrições **Ausentes** abaixo de Todos os Exibições no Controlador de Modo de **Exibição de Boas-vindas.**</span><span class="sxs-lookup"><span data-stu-id="053ba-226">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="053ba-227">A cena do calendário deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="053ba-227">The calendar scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout da cena calendário](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="053ba-229">Criar indicador de atividade</span><span class="sxs-lookup"><span data-stu-id="053ba-229">Create activity indicator</span></span>

1. <span data-ttu-id="053ba-230">Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `SpinnerViewController` .</span><span class="sxs-lookup"><span data-stu-id="053ba-230">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="053ba-231">Escolha **UIViewController** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="053ba-231">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="053ba-232">Abra **SpinnerViewController.h** e adicione o seguinte código à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="053ba-232">Open **SpinnerViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    - (void) startWithContainer:(UIViewController*) container;
    - (void) stop;
    ```

1. <span data-ttu-id="053ba-233">Abra **SpinnerViewController.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="053ba-233">Open **SpinnerViewController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.m" id="SpinnerViewSnippet":::

## <a name="test-the-app"></a><span data-ttu-id="053ba-234">O aplicativo de teste</span><span class="sxs-lookup"><span data-stu-id="053ba-234">Test the app</span></span>

<span data-ttu-id="053ba-235">Salve suas alterações e iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="053ba-235">Save your changes and launch the app.</span></span> <span data-ttu-id="053ba-236">Você deve ser capaz de se  mover entre as telas usando os botões Entrar e **Sair** e a barra de guias.</span><span class="sxs-lookup"><span data-stu-id="053ba-236">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Capturas de tela do aplicativo](./images/app-screens.png)
