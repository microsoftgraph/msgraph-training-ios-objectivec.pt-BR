<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="57bfc-101">Comece criando um novo projeto Swift.</span><span class="sxs-lookup"><span data-stu-id="57bfc-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="57bfc-102">Abra o Xcode.</span><span class="sxs-lookup"><span data-stu-id="57bfc-102">Open Xcode.</span></span> <span data-ttu-id="57bfc-103">No menu **arquivo** , selecione **novo**e, em seguida, **projeto**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="57bfc-104">Escolha o modelo de **aplicativo exibir único** e selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-104">Choose the **Single View App** template and select **Next**.</span></span>

    ![Uma captura de tela da caixa de diálogo de seleção do modelo do Xcode](./images/xcode-select-template.png)

1. <span data-ttu-id="57bfc-106">Defina o **nome** do produto `GraphTutorial` como e o **idioma** para **Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Objective-C**.</span></span>
1. <span data-ttu-id="57bfc-107">Preencha os campos restantes e selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="57bfc-108">Escolha um local para o projeto e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="57bfc-109">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="57bfc-109">Install dependencies</span></span>

<span data-ttu-id="57bfc-110">Antes de prosseguir, instale algumas dependências adicionais que serão usadas posteriormente.</span><span class="sxs-lookup"><span data-stu-id="57bfc-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="57bfc-111">[Biblioteca de autenticação da Microsoft (MSAL) para IOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) para autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57bfc-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="57bfc-112">[SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="57bfc-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="57bfc-113">[SDK de modelos do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) para objetos fortemente tipados que representam os recursos do Microsoft Graph, como usuários ou eventos.</span><span class="sxs-lookup"><span data-stu-id="57bfc-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="57bfc-114">Encerre o Xcode.</span><span class="sxs-lookup"><span data-stu-id="57bfc-114">Quit Xcode.</span></span>
1. <span data-ttu-id="57bfc-115">Abra o terminal e altere o diretório para o local do seu projeto do **GraphTutorial** .</span><span class="sxs-lookup"><span data-stu-id="57bfc-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="57bfc-116">Execute o comando a seguir para criar um Podfile.</span><span class="sxs-lookup"><span data-stu-id="57bfc-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="57bfc-117">Abra o Podfile e adicione as seguintes linhas logo após a `use_frameworks!` linha.</span><span class="sxs-lookup"><span data-stu-id="57bfc-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.0.2'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="57bfc-118">Salve o Podfile e execute o seguinte comando para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="57bfc-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="57bfc-119">Quando o comando for concluído, abra o **GraphTutorial. xcworkspace** recém-criado no Xcode.</span><span class="sxs-lookup"><span data-stu-id="57bfc-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="57bfc-120">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="57bfc-120">Design the app</span></span>

<span data-ttu-id="57bfc-121">Nesta seção, você criará os modos de exibição do aplicativo: uma página de entrada, um navegador de barra de guias, uma página de boas-vindas e uma página de calendário.</span><span class="sxs-lookup"><span data-stu-id="57bfc-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="57bfc-122">Você também criará uma sobreposição de indicador de atividade.</span><span class="sxs-lookup"><span data-stu-id="57bfc-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="57bfc-123">Criar página de entrada</span><span class="sxs-lookup"><span data-stu-id="57bfc-123">Create sign in page</span></span>

1. <span data-ttu-id="57bfc-124">Expanda a pasta **GraphTutorial** no Xcode e, em seguida, selecione o arquivo **ViewController. m** .</span><span class="sxs-lookup"><span data-stu-id="57bfc-124">Expand the **GraphTutorial** folder in Xcode, then select the **ViewController.m** file.</span></span>
1. <span data-ttu-id="57bfc-125">No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.m`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.m`.</span></span>

    ![Uma captura de tela do Inspetor de arquivos](./images/rename-file.png)

1. <span data-ttu-id="57bfc-127">Abra **SignInViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="57bfc-127">Open **SignInViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="57bfc-128">Selecione o arquivo **ViewController. h** .</span><span class="sxs-lookup"><span data-stu-id="57bfc-128">Select the **ViewController.h** file.</span></span>
1. <span data-ttu-id="57bfc-129">No **Inspetor de arquivos**, altere o **nome** do arquivo para `SignInViewController.h`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-129">In the **File Inspector**, change the **Name** of the file to `SignInViewController.h`.</span></span>
1. <span data-ttu-id="57bfc-130">Abra **SignInViewController. h** e altere todas as instâncias `ViewController` de `SignInViewController`para.</span><span class="sxs-lookup"><span data-stu-id="57bfc-130">Open **SignInViewController.h** and change all instances of `ViewController` to `SignInViewController`.</span></span>

1. <span data-ttu-id="57bfc-131">Abra o arquivo **Main. Storyboard** .</span><span class="sxs-lookup"><span data-stu-id="57bfc-131">Open the **Main.storyboard** file.</span></span>
1. <span data-ttu-id="57bfc-132">Expanda **Exibir controle cena**e selecione **Exibir controlador**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-132">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Uma captura de tela do Xcode com o controlador de modo de exibição selecionado](./images/storyboard-select-view-controller.png)

1. <span data-ttu-id="57bfc-134">Selecione o **Inspetor de identidade**e, em seguida, altere o menu suspenso de **classe** para **SignInViewController**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-134">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Uma captura de tela do Inspetor de identidade](./images/change-class.png)

1. <span data-ttu-id="57bfc-136">Selecione a **biblioteca**e arraste um **botão** para o **controlador de exibição de entrada**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-136">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Uma captura de tela da biblioteca no Xcode](./images/add-button-to-view.png)

1. <span data-ttu-id="57bfc-138">Com o botão selecionado, selecione o **Inspetor de atributos** e altere o **título** do botão para `Sign In`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-138">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Uma captura de tela do campo título no Inspetor de atributos no Xcode](./images/set-button-title.png)

1. <span data-ttu-id="57bfc-140">Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-140">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="57bfc-141">Em **ações recebidas**, arraste o círculo não preenchido ao lado de **entrar** no botão.</span><span class="sxs-lookup"><span data-stu-id="57bfc-141">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="57bfc-142">Selecione **retoque** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="57bfc-142">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Uma captura de tela de arrastar a ação de entrada para o botão no Xcode](./images/connect-sign-in-button.png)

1. <span data-ttu-id="57bfc-144">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todas as exibições na controladora de exibição de entrada**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-144">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Sign In View Controller**.</span></span>

### <a name="create-tab-bar"></a><span data-ttu-id="57bfc-145">Criar barra de guias</span><span class="sxs-lookup"><span data-stu-id="57bfc-145">Create tab bar</span></span>

1. <span data-ttu-id="57bfc-146">Selecione a **biblioteca**e, em seguida, arraste um **controlador da barra de guias** para o storyboard.</span><span class="sxs-lookup"><span data-stu-id="57bfc-146">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="57bfc-147">Selecione o **controlador de exibição de entrada**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-147">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="57bfc-148">Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador da barra de guias** no storyboard.</span><span class="sxs-lookup"><span data-stu-id="57bfc-148">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="57bfc-149">Selecione **Mostrar modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="57bfc-149">Select **Present Modally** in the pop-up menu.</span></span>

    ![Uma captura de tela de arrastar um segue manual para o novo controlador de barra de guias no Xcode](./images/add-segue.png)

1. <span data-ttu-id="57bfc-151">Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-151">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="57bfc-152">Definir o campo **identificador** como `userSignedIn`e definir a **apresentação** como **tela inteira**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-152">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Uma captura de tela do campo identificador no Inspetor de atributos no Xcode](./images/set-segue-identifier.png)

1. <span data-ttu-id="57bfc-154">Selecione o **Item 1 cena**e, em seguida, selecione o **Inspetor de conexões**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-154">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="57bfc-155">Em **segues disparado**, arraste o círculo não preenchido ao lado de **manual** no **controlador de exibição de entrada** no storyboard.</span><span class="sxs-lookup"><span data-stu-id="57bfc-155">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="57bfc-156">Selecione **Mostrar modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="57bfc-156">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="57bfc-157">Selecione o segue que você acabou de adicionar e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-157">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="57bfc-158">Definir o campo **identificador** como `userSignedOut`e definir a **apresentação** como **tela inteira**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-158">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="57bfc-159">Criar página de boas-vindas</span><span class="sxs-lookup"><span data-stu-id="57bfc-159">Create welcome page</span></span>

1. <span data-ttu-id="57bfc-160">Selecione o arquivo **assets. xcassets** .</span><span class="sxs-lookup"><span data-stu-id="57bfc-160">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="57bfc-161">No menu **Editor** , selecione **Adicionar ativos**e, em seguida, **novo conjunto de imagens**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-161">On the **Editor** menu, select **Add Assets**, then **New Image Set**.</span></span>
1. <span data-ttu-id="57bfc-162">Selecione o novo ativo de **imagem** e use o **Inspetor de atributos** para definir seu `DefaultUserPhoto` **nome** como.</span><span class="sxs-lookup"><span data-stu-id="57bfc-162">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="57bfc-163">Adicione qualquer imagem que você deseja que sirva como uma foto de perfil de usuário padrão.</span><span class="sxs-lookup"><span data-stu-id="57bfc-163">Add any image you like to serve as a default user profile photo.</span></span>

    ![Uma captura de tela do modo de exibição ativo do conjunto de imagens no Xcode](./images/add-default-user-photo.png)

1. <span data-ttu-id="57bfc-165">Crie um novo arquivo de **classe Touch** do Cocoa \*\*\*\* na pasta GraphTutorial `WelcomeViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="57bfc-165">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="57bfc-166">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="57bfc-166">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="57bfc-167">Abra **WelcomeViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="57bfc-167">Open **WelcomeViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UIImageView *userProfilePhoto;
    @property (nonatomic) IBOutlet UILabel *userDisplayName;
    @property (nonatomic) IBOutlet UILabel *userEmail;
    ```

1. <span data-ttu-id="57bfc-168">Abra **WelcomeViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="57bfc-168">Open **WelcomeViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="57bfc-169">Abra **Main. Storyboard**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-169">Open **Main.storyboard**.</span></span> <span data-ttu-id="57bfc-170">Selecione a **cena item 1**e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-170">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="57bfc-171">Altere o valor da **classe** para **WelcomeViewController**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-171">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="57bfc-172">Usando a **biblioteca**, adicione os itens a seguir à **cena do item 1**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-172">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="57bfc-173">Um **modo de exibição de imagem**</span><span class="sxs-lookup"><span data-stu-id="57bfc-173">One **Image View**</span></span>
    - <span data-ttu-id="57bfc-174">Dois **Rótulos**</span><span class="sxs-lookup"><span data-stu-id="57bfc-174">Two **Labels**</span></span>
    - <span data-ttu-id="57bfc-175">Um **botão**</span><span class="sxs-lookup"><span data-stu-id="57bfc-175">One **Button**</span></span>

1. <span data-ttu-id="57bfc-176">Selecione o modo de exibição imagem e, em seguida, selecione o **Inspetor de tamanho**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-176">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="57bfc-177">Defina a **largura** e a **altura** como 196.</span><span class="sxs-lookup"><span data-stu-id="57bfc-177">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="57bfc-178">Selecione o segundo rótulo e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-178">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="57bfc-179">Altere a **cor** para **cor cinza escuro**e altere a **fonte** para **System 12,0**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-179">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="57bfc-180">Selecione o botão e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-180">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="57bfc-181">Altere o **título** para `Sign Out`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-181">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="57bfc-182">Usando o **Inspetor de conexões**, faça as seguintes conexões.</span><span class="sxs-lookup"><span data-stu-id="57bfc-182">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="57bfc-183">Vincule a saída **UserDisplayName** ao primeiro rótulo.</span><span class="sxs-lookup"><span data-stu-id="57bfc-183">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="57bfc-184">Vincule a saída **UserEmail** ao segundo rótulo.</span><span class="sxs-lookup"><span data-stu-id="57bfc-184">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="57bfc-185">Vincule a tomada **userProfilePhoto** à visualização de imagem.</span><span class="sxs-lookup"><span data-stu-id="57bfc-185">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="57bfc-186">Vincule a ação de **SignOut** recebida à **retoque**do botão dentro.</span><span class="sxs-lookup"><span data-stu-id="57bfc-186">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="57bfc-187">Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-187">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="57bfc-188">Altere o **título** para `Me`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-188">Change the **Title** to `Me`.</span></span>
1. <span data-ttu-id="57bfc-189">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**</span><span class="sxs-lookup"><span data-stu-id="57bfc-189">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="57bfc-190">A cena de boas-vindas deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="57bfc-190">The welcome scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout da cena de boas-vindas](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="57bfc-192">Criar página de calendário</span><span class="sxs-lookup"><span data-stu-id="57bfc-192">Create calendar page</span></span>

1. <span data-ttu-id="57bfc-193">Crie um novo arquivo de **classe Touch** do Cocoa \*\*\*\* na pasta GraphTutorial `CalendarViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="57bfc-193">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="57bfc-194">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="57bfc-194">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="57bfc-195">Abra **CalendarViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="57bfc-195">Open **CalendarViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    @property (nonatomic) IBOutlet UITextView *calendarJSON;
    ```

1. <span data-ttu-id="57bfc-196">Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="57bfc-196">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="57bfc-197">Abra **Main. Storyboard**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-197">Open **Main.storyboard**.</span></span> <span data-ttu-id="57bfc-198">Selecione a **cena item 2**e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-198">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="57bfc-199">Altere o valor da **classe** para **CalendarViewController**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-199">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="57bfc-200">Usando a **biblioteca**, adicione um **modo de exibição de texto** à **cena do item 2**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-200">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="57bfc-201">Selecione o modo de exibição de texto que você acabou de adicionar.</span><span class="sxs-lookup"><span data-stu-id="57bfc-201">Select the text view you just added.</span></span> <span data-ttu-id="57bfc-202">No **Editor**, escolha **incorporar no**e modo de **rolagem**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-202">On the **Editor**, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="57bfc-203">Usando o **Inspetor de conexões**, conecte a tomada **calendarJSON** à visualização de texto.</span><span class="sxs-lookup"><span data-stu-id="57bfc-203">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. 1. <span data-ttu-id="57bfc-204">Selecione o item da barra de guias na parte inferior da cena e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-204">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="57bfc-205">Altere o **título** para `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-205">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="57bfc-206">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**</span><span class="sxs-lookup"><span data-stu-id="57bfc-206">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="57bfc-207">A cena do calendário deve ser semelhante a esta quando você terminar.</span><span class="sxs-lookup"><span data-stu-id="57bfc-207">The calendar scene should look similar to this once you're done.</span></span>

![Uma captura de tela do layout de cena do calendário](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="57bfc-209">Criar indicador de atividade</span><span class="sxs-lookup"><span data-stu-id="57bfc-209">Create activity indicator</span></span>

1. <span data-ttu-id="57bfc-210">Crie um novo arquivo de **classe Touch** do Cocoa \*\*\*\* na pasta GraphTutorial `SpinnerViewController`chamada.</span><span class="sxs-lookup"><span data-stu-id="57bfc-210">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="57bfc-211">Escolha **UIViewController** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="57bfc-211">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="57bfc-212">Abra **SpinnerViewController. h** e adicione o código a seguir dentro `@interface` da declaração.</span><span class="sxs-lookup"><span data-stu-id="57bfc-212">Open **SpinnerViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objc
    - (void) startWithContainer:(UIViewController*) container;
    - (void) stop;
    ```

1. <span data-ttu-id="57bfc-213">Abra **SpinnerViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="57bfc-213">Open **SpinnerViewController.m** and replace its contents with the following code.</span></span>

    ```objc
    #import "SpinnerViewController.h"

    @interface SpinnerViewController ()
    @property (nonatomic) UIActivityIndicatorView* spinner;
    @end

    @implementation SpinnerViewController

    - (void)viewDidLoad {
        [super viewDidLoad];

        _spinner = [[UIActivityIndicatorView alloc] initWithActivityIndicatorStyle:
                    UIActivityIndicatorViewStyleWhiteLarge];

        self.view.backgroundColor = [UIColor colorWithWhite:0 alpha:0.7];
        [self.view addSubview:_spinner];

        _spinner.translatesAutoresizingMaskIntoConstraints = false;
        [_spinner startAnimating];

        [_spinner.centerXAnchor constraintEqualToAnchor:self.view.centerXAnchor].active = true;
        [_spinner.centerYAnchor constraintEqualToAnchor:self.view.centerYAnchor].active = true;
    }

    - (void) startWithContainer:(UIViewController *)container {
        [container addChildViewController:self];
        self.view.frame = container.view.frame;
        [container.view addSubview:self.view];
        [self didMoveToParentViewController:container];
    }

    - (void) stop {
        [self willMoveToParentViewController:nil];
        [self.view removeFromSuperview];
        [self removeFromParentViewController];
    }

    @end
    ```

## <a name="test-the-app"></a><span data-ttu-id="57bfc-214">O aplicativo de teste</span><span class="sxs-lookup"><span data-stu-id="57bfc-214">Test the app</span></span>

<span data-ttu-id="57bfc-215">Salve suas alterações e inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="57bfc-215">Save your changes and launch the app.</span></span> <span data-ttu-id="57bfc-216">Você deve ser capaz de se mover entre as telas usando \*\*\*\* os botões entrar **e sair e** a barra de guias.</span><span class="sxs-lookup"><span data-stu-id="57bfc-216">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Capturas de tela do aplicativo](./images/app-screens.png)
