<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="de1a8-101">Comece criando um novo projeto Swift.</span><span class="sxs-lookup"><span data-stu-id="de1a8-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="de1a8-102">Abra o Xcode.</span><span class="sxs-lookup"><span data-stu-id="de1a8-102">Open Xcode.</span></span> <span data-ttu-id="de1a8-103">No menu **arquivo** , selecione **novo**e, em seguida, **projeto**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="de1a8-104">Escolha o modelo de **aplicativo exibir único** e selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-104">Choose the **Single View App** template and select **Next**.</span></span>

    ![Uma captura de tela da caixa de diálogo de seleção do modelo do Xcode](./images/xcode-select-template.png)

1. <span data-ttu-id="de1a8-106">Defina o **nome** do produto `GraphTutorial` como e o **idioma** para **Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Objective-C**.</span></span>
1. <span data-ttu-id="de1a8-107">Preencha os campos restantes e selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="de1a8-108">Escolha um local para o projeto e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="de1a8-109">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="de1a8-109">Install dependencies</span></span>

<span data-ttu-id="de1a8-110">Antes de prosseguir, instale algumas dependências adicionais que serão usadas posteriormente.</span><span class="sxs-lookup"><span data-stu-id="de1a8-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="de1a8-111">[Biblioteca de autenticação da Microsoft (MSAL) para IOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) para autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de1a8-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="de1a8-112">[SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="de1a8-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="de1a8-113">[SDK de modelos do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) para objetos fortemente tipados que representam os recursos do Microsoft Graph, como usuários ou eventos.</span><span class="sxs-lookup"><span data-stu-id="de1a8-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="de1a8-114">Encerre o Xcode.</span><span class="sxs-lookup"><span data-stu-id="de1a8-114">Quit Xcode.</span></span>
1. <span data-ttu-id="de1a8-115">Abra o terminal e altere o diretório para o local do seu projeto do **GraphTutorial** .</span><span class="sxs-lookup"><span data-stu-id="de1a8-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="de1a8-116">Execute o comando a seguir para criar um Podfile.</span><span class="sxs-lookup"><span data-stu-id="de1a8-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="de1a8-117">Abra o Podfile e adicione as seguintes linhas logo após a `use_frameworks!` linha.</span><span class="sxs-lookup"><span data-stu-id="de1a8-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.1.1'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="de1a8-118">Salve o Podfile e execute o seguinte comando para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="de1a8-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="de1a8-119">Quando o comando for concluído, abra o **GraphTutorial. xcworkspace** recém-criado no Xcode.</span><span class="sxs-lookup"><span data-stu-id="de1a8-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="de1a8-120">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="de1a8-120">Design the app</span></span>

<span data-ttu-id="de1a8-121">Nesta seção, você criará os modos de exibição do aplicativo: uma página de entrada, um navegador de barra de guias, uma página de boas-vindas e uma página de calendário.</span><span class="sxs-lookup"><span data-stu-id="de1a8-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="de1a8-122">Você também criará uma sobreposição de indicador de atividade.</span><span class="sxs-lookup"><span data-stu-id="de1a8-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="de1a8-123">Criar página de entrada</span><span class="sxs-lookup"><span data-stu-id="de1a8-123">Create sign in page</span></span>

1. <span data-ttu-id="de1a8-124">Expanda a pasta **GraphTutorial** no Xcode e, em seguida, selecione o arquivo **ViewController. m** .</span><span class="sxs-lookup"><span data-stu-id="de1a8-124">Expand the **GraphTutorial** folder in Xcode, then select the **ViewController.m** file.</span></span>
1. <span data-ttu-id="de1a8-125">No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.m`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.m`.</span></span>

    ![Uma captura de tela do Inspetor de arquivos](./images/rename-file.png)

1. <span data-ttu-id="de1a8-127">Abra **SignInViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="de1a8-127">Open **SignInViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="de1a8-128">Selecione o arquivo **ViewController. h** .</span><span class="sxs-lookup"><span data-stu-id="de1a8-128">Select the **ViewController.h** file.</span></span>
1. <span data-ttu-id="de1a8-129">No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.h`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-129">In the **File Inspector**, change the **Name** of the file to `SignInViewController.h`.</span></span>
1. <span data-ttu-id="de1a8-130">Abra **SignInViewController. h** e altere todas as instâncias `ViewController` de `SignInViewController`para.</span><span class="sxs-lookup"><span data-stu-id="de1a8-130">Open **SignInViewController.h** and change all instances of `ViewController` to `SignInViewController`.</span></span>

1. <span data-ttu-id="de1a8-131">Abra o arquivo **Main. Storyboard** .</span><span class="sxs-lookup"><span data-stu-id="de1a8-131">Open the **Main.storyboard** file.</span></span>
1. <span data-ttu-id="de1a8-132">Expanda **Exibir controle cena**e selecione **Exibir controlador**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-132">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Uma captura de tela do Xcode com o controlador de modo de exibição selecionado](./images/storyboard-select-view-controller.png)

1. <span data-ttu-id="de1a8-134">Selecione o **Inspetor de identidade**e, em seguida, altere o menu suspenso de **classe** para **SignInViewController**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-134">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Uma captura de tela do Inspetor de identidade](./images/change-class.png)

1. <span data-ttu-id="de1a8-136">Selecione a **biblioteca**e arraste um **botão** para o **controlador de exibição de entrada**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-136">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Uma captura de tela da biblioteca no Xcode](./images/add-button-to-view.png)

1. <span data-ttu-id="de1a8-138">Com o botão selecionado, selecione o **Inspetor de atributos** e altere o **título** do botão para `Sign In`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-138">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Uma captura de tela do campo título no Inspetor de atributos no Xcode](./images/set-button-title.png)

1. <span data-ttu-id="de1a8-140">Com o botão selecionado, selecione o botão **alinhar** na parte inferior do storyboard.</span><span class="sxs-lookup"><span data-stu-id="de1a8-140">With the button selected, select the **Align** button at the bottom of the storyboard.</span></span> <span data-ttu-id="de1a8-141">Selecione as duas opções **horizontalmente em contêiner** e **verticalmente em restrições de contêiner** , deixe seus valores como 0 e selecione **adicionar duas restrições**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-141">Select both the **Horizontally in container** and **Vertically in container** constraints, leave their values as 0, then select **Add 2 constraints**.</span></span>

    ![Uma captura de tela das configurações de restrições de alinhamento no Xcode](./images/add-alignment-constraints.png)

1. <span data-ttu-id="de1a8-143">Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-143">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="de1a8-144">Em **ações recebidas**, arraste o círculo não preenchido ao lado de **entrar** no botão.</span><span class="sxs-lookup"><span data-stu-id="de1a8-144">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="de1a8-145">Selecione **retoque** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="de1a8-145">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Uma captura de tela de arrastar a ação de entrada para o botão no Xcode](./images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a><span data-ttu-id="de1a8-147">Criar barra de guias</span><span class="sxs-lookup"><span data-stu-id="de1a8-147">Create tab bar</span></span>

1. <span data-ttu-id="de1a8-148">Selecione a **biblioteca**e, em seguida, arraste um **controlador da barra de guias** para o storyboard.</span><span class="sxs-lookup"><span data-stu-id="de1a8-148">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="de1a8-149">Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-149">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="de1a8-150">Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador da barra de guias** no storyboard.</span><span class="sxs-lookup"><span data-stu-id="de1a8-150">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="de1a8-151">Selecione **Mostrar modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="de1a8-151">Select **Present Modally** in the pop-up menu.</span></span>

    ![Uma captura de tela de arrastar um segue manual para o novo controlador de barra de guias no Xcode](./images/add-segue.png)

1. <span data-ttu-id="de1a8-153">Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-153">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="de1a8-154">Definir o campo **identificador** como `userSignedIn`e definir a **apresentação** como **tela inteira**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-154">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Uma captura de tela do campo identificador no Inspetor de atributos no Xcode](./images/set-segue-identifier.png)

1. <span data-ttu-id="de1a8-156">Selecione o **Item 1 cena**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-156">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="de1a8-157">Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador de exibição de entrada** no storyboard.</span><span class="sxs-lookup"><span data-stu-id="de1a8-157">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="de1a8-158">Selecione **Mostrar modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="de1a8-158">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="de1a8-159">Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-159">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="de1a8-160">Definir o campo **identificador** como `userSignedOut`e definir a **apresentação** como **tela inteira**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-160">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="de1a8-161">Criar página de boas-vindas</span><span class="sxs-lookup"><span data-stu-id="de1a8-161">Create welcome page</span></span>

1. <span data-ttu-id="de1a8-162">Selecione o arquivo **assets. xcassets** .</span><span class="sxs-lookup"><span data-stu-id="de1a8-162">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="de1a8-163">No menu **Editor** , selecione **Adicionar ativos**e, em seguida, **novo conjunto de imagens**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-163">On the **Editor** menu, select **Add Assets**, then **New Image Set**.</span></span>
1. <span data-ttu-id="de1a8-164">Selecione o novo ativo de **imagem** e use o **Inspetor de atributos** para definir seu `DefaultUserPhoto` **nome** como.</span><span class="sxs-lookup"><span data-stu-id="de1a8-164">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="de1a8-165">Adicione qualquer imagem que você deseja que sirva como uma foto de perfil de usuário padrão.</span><span class="sxs-lookup"><span data-stu-id="de1a8-165">Add any image you like to serve as a default user profile photo.</span></span>

    ![Uma captura de tela do modo de exibição ativo do conjunto de imagens no Xcode](./images/add-default-user-photo.png)

1. <span data-ttu-id="de1a8-167">Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `WelcomeViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="de1a8-167">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="de1a8-168">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="de1a8-168">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="de1a8-169">Abra **WelcomeViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="de1a8-169">Open **WelcomeViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UIImageView *userProfilePhoto;
    @property (nonatomic) IBOutlet UILabel *userDisplayName;
    @property (nonatomic) IBOutlet UILabel *userEmail;
    ```

1. <span data-ttu-id="de1a8-170">Abra **WelcomeViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="de1a8-170">Open **WelcomeViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="de1a8-171">Abra **Main. Storyboard**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-171">Open **Main.storyboard**.</span></span> <span data-ttu-id="de1a8-172">Selecione a **cena item 1**e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-172">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="de1a8-173">Altere o valor da **classe** para **WelcomeViewController**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-173">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="de1a8-174">Usando a **biblioteca**, adicione os itens a seguir à **cena do item 1**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-174">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="de1a8-175">Um **modo de exibição de imagem**</span><span class="sxs-lookup"><span data-stu-id="de1a8-175">One **Image View**</span></span>
    - <span data-ttu-id="de1a8-176">Dois **Rótulos**</span><span class="sxs-lookup"><span data-stu-id="de1a8-176">Two **Labels**</span></span>
    - <span data-ttu-id="de1a8-177">Um **botão**</span><span class="sxs-lookup"><span data-stu-id="de1a8-177">One **Button**</span></span>

1. <span data-ttu-id="de1a8-178">Usando o **Inspetor de conexões**, faça as seguintes conexões.</span><span class="sxs-lookup"><span data-stu-id="de1a8-178">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="de1a8-179">Vincule a saída **UserDisplayName** ao primeiro rótulo.</span><span class="sxs-lookup"><span data-stu-id="de1a8-179">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="de1a8-180">Vincule a saída **UserEmail** ao segundo rótulo.</span><span class="sxs-lookup"><span data-stu-id="de1a8-180">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="de1a8-181">Vincule a tomada **userProfilePhoto** à visualização de imagem.</span><span class="sxs-lookup"><span data-stu-id="de1a8-181">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="de1a8-182">Vincule a ação de **SignOut** recebida à **retoque**do botão dentro.</span><span class="sxs-lookup"><span data-stu-id="de1a8-182">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="de1a8-183">Selecione o modo de exibição imagem e, em seguida, selecione o **Inspetor de tamanho**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-183">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="de1a8-184">Defina a **largura** e a **altura** como 196.</span><span class="sxs-lookup"><span data-stu-id="de1a8-184">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="de1a8-185">Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="de1a8-185">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="de1a8-186">Use o botão **adicionar novas restrições** (ao lado do botão **alinhar** ) para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="de1a8-186">Use the **Add New Constraints** button (next to the **Align** button) to add the following constraints:</span></span>

    - <span data-ttu-id="de1a8-187">Alinhar parte superior a: área segura, valor: 0</span><span class="sxs-lookup"><span data-stu-id="de1a8-187">Align Top to: Safe Area, value: 0</span></span>
    - <span data-ttu-id="de1a8-188">Espaço inferior para: nome de exibição do usuário, valor: Standard</span><span class="sxs-lookup"><span data-stu-id="de1a8-188">Bottom Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="de1a8-189">Altura, valor: 196</span><span class="sxs-lookup"><span data-stu-id="de1a8-189">Height, value: 196</span></span>
    - <span data-ttu-id="de1a8-190">Largura, valor: 196</span><span class="sxs-lookup"><span data-stu-id="de1a8-190">Width, value: 196</span></span>

    ![Uma captura de tela das novas configurações de restrições no Xcode](./images/add-new-constraints.png)

1. <span data-ttu-id="de1a8-192">Selecione o primeiro rótulo e, em seguida, use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="de1a8-192">Select the first label, then use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="de1a8-193">Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="de1a8-193">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="de1a8-194">Espaço principal para: foto de perfil de usuário, valor: Standard</span><span class="sxs-lookup"><span data-stu-id="de1a8-194">Top Space to: User Profile Photo, value: Standard</span></span>
    - <span data-ttu-id="de1a8-195">Espaço inferior para: email do usuário, valor: padrão</span><span class="sxs-lookup"><span data-stu-id="de1a8-195">Bottom Space to: User Email, value: Standard</span></span>

1. <span data-ttu-id="de1a8-196">Selecione o segundo rótulo e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-196">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="de1a8-197">Altere a **cor** para **cor cinza escuro**e altere a **fonte** para **System 12,0**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-197">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="de1a8-198">Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="de1a8-198">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="de1a8-199">Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="de1a8-199">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="de1a8-200">Espaço principal para: nome de exibição do usuário, valor: Standard</span><span class="sxs-lookup"><span data-stu-id="de1a8-200">Top Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="de1a8-201">Espaço inferior para: sair, valor: 14</span><span class="sxs-lookup"><span data-stu-id="de1a8-201">Bottom Space to: Sign Out, value: 14</span></span>

1. <span data-ttu-id="de1a8-202">Selecione o botão e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-202">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="de1a8-203">Altere o **título** para `Sign Out`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-203">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="de1a8-204">Use o botão **alinhar** para adicionar a restrição de **contêiner horizontalmente** com um valor de 0.</span><span class="sxs-lookup"><span data-stu-id="de1a8-204">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="de1a8-205">Use o botão **adicionar novas restrições** para adicionar as seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="de1a8-205">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="de1a8-206">Espaço principal para: email do usuário, valor: 14</span><span class="sxs-lookup"><span data-stu-id="de1a8-206">Top Space to: User Email, value: 14</span></span>

1. <span data-ttu-id="de1a8-207">Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-207">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="de1a8-208">Altere o **título** para `Me`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-208">Change the **Title** to `Me`.</span></span>
1. <span data-ttu-id="de1a8-209">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**</span><span class="sxs-lookup"><span data-stu-id="de1a8-209">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="de1a8-210">A cena de boas-vindas deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="de1a8-210">The welcome scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout da cena de boas-vindas](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="de1a8-212">Criar página de calendário</span><span class="sxs-lookup"><span data-stu-id="de1a8-212">Create calendar page</span></span>

1. <span data-ttu-id="de1a8-213">Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `CalendarViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="de1a8-213">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="de1a8-214">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="de1a8-214">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="de1a8-215">Abra **CalendarViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="de1a8-215">Open **CalendarViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UITextView *calendarJSON;
    ```

1. <span data-ttu-id="de1a8-216">Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="de1a8-216">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="de1a8-217">Abra **Main. Storyboard**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-217">Open **Main.storyboard**.</span></span> <span data-ttu-id="de1a8-218">Selecione a **cena item 2**e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-218">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="de1a8-219">Altere o valor da **classe** para **CalendarViewController**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-219">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="de1a8-220">Usando a **biblioteca**, adicione um **modo de exibição de texto** à **cena do item 2**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-220">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="de1a8-221">Selecione o modo de exibição de texto que você acabou de adicionar.</span><span class="sxs-lookup"><span data-stu-id="de1a8-221">Select the text view you just added.</span></span> <span data-ttu-id="de1a8-222">No **Editor**, escolha **incorporar no**e modo de **rolagem**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-222">On the **Editor**, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="de1a8-223">Usando o **Inspetor de conexões**, conecte a tomada **calendarJSON** à visualização de texto.</span><span class="sxs-lookup"><span data-stu-id="de1a8-223">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. <span data-ttu-id="de1a8-224">Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="de1a8-224">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="de1a8-225">Altere o **título** para `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="de1a8-225">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="de1a8-226">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**</span><span class="sxs-lookup"><span data-stu-id="de1a8-226">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="de1a8-227">A cena do calendário deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="de1a8-227">The calendar scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout de cena do calendário](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="de1a8-229">Criar indicador de atividade</span><span class="sxs-lookup"><span data-stu-id="de1a8-229">Create activity indicator</span></span>

1. <span data-ttu-id="de1a8-230">Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** na pasta GraphTutorial `SpinnerViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="de1a8-230">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="de1a8-231">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="de1a8-231">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="de1a8-232">Abra **SpinnerViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="de1a8-232">Open **SpinnerViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    - (void) startWithContainer:(UIViewController*) container;
    - (void) stop;
    ```

1. <span data-ttu-id="de1a8-233">Abra **SpinnerViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="de1a8-233">Open **SpinnerViewController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.m" id="SpinnerViewSnippet":::

## <a name="test-the-app"></a><span data-ttu-id="de1a8-234">O aplicativo de teste</span><span class="sxs-lookup"><span data-stu-id="de1a8-234">Test the app</span></span>

<span data-ttu-id="de1a8-235">Salve suas alterações e inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="de1a8-235">Save your changes and launch the app.</span></span> <span data-ttu-id="de1a8-236">Você deve ser capaz de se mover entre as telas usando **Sign In** os botões entrar **e sair e** a barra de guias.</span><span class="sxs-lookup"><span data-stu-id="de1a8-236">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Capturas de tela do aplicativo](./images/app-screens.png)
