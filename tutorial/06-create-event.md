<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b2575-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="b2575-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="b2575-102">Abra **GraphManager.h** e adicione o seguinte código acima da `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="b2575-102">Open **GraphManager.h** and add the following code above the `@interface` declaration.</span></span>

    ```objc
    typedef void (^CreateEventCompletionBlock)(MSGraphEvent* _Nullable event,
                                               NSError* _Nullable error);
    ```

1. <span data-ttu-id="b2575-103">Adicione o código a seguir à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="b2575-103">Add the following code to the `@interface` declaration.</span></span>

    ```objc
    - (void) createEventWithSubject: (NSString*) subject
                           andStart: (NSDate*) start
                             andEnd: (NSDate*) end
                       andAttendees: (NSArray<NSString*>* _Nullable) attendees
                            andBody: (NSString* _Nullable) body
                 andCompletionBlock: (CreateEventCompletionBlock) completion;
    ```

1. <span data-ttu-id="b2575-104">Abra **GraphManager.m** e adicione a função a seguir para criar um novo evento no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="b2575-104">Open **GraphManager.m** and add the following function to create a new event on the user's calendar.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphManager.m" id="CreateEventSnippet":::

1. <span data-ttu-id="b2575-105">Crie um novo **arquivo de classe Cocoa Touch** na pasta **GraphTutorial** chamada `NewEventViewController` .</span><span class="sxs-lookup"><span data-stu-id="b2575-105">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `NewEventViewController`.</span></span> <span data-ttu-id="b2575-106">Escolha **UIViewController** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="b2575-106">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="b2575-107">Abra **NewEventViewController.h** e adicione o código a seguir à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="b2575-107">Open **NewEventViewController.h** and add the following code inside the `@interface` declaration.</span></span>

    ```objectivec
    @property (nonatomic) IBOutlet UITextField* subject;
    @property (nonatomic) IBOutlet UITextField* attendees;
    @property (nonatomic) IBOutlet UIDatePicker* start;
    @property (nonatomic) IBOutlet UIDatePicker* end;
    @property (nonatomic) IBOutlet UITextView* body;
    ```

1. <span data-ttu-id="b2575-108">Abra **NewEventController.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b2575-108">Open **NewEventController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/NewEventViewController.m" id="NewEventViewControllerSnippet":::

1. <span data-ttu-id="b2575-109">Abra **Main.storyboard**.</span><span class="sxs-lookup"><span data-stu-id="b2575-109">Open **Main.storyboard**.</span></span> <span data-ttu-id="b2575-110">Use a **Biblioteca para** arrastar um Controlador **de Exibição** para o storyboard.</span><span class="sxs-lookup"><span data-stu-id="b2575-110">Use the **Library** to drag a **View Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="b2575-111">Usando a **Biblioteca,** adicione uma **Barra de Navegação** ao controlador de exibição.</span><span class="sxs-lookup"><span data-stu-id="b2575-111">Using the **Library**, add a **Navigation Bar** to the view controller.</span></span>
1. <span data-ttu-id="b2575-112">Clique duas vezes no **título** na barra de navegação e atualize-o para `New Event` .</span><span class="sxs-lookup"><span data-stu-id="b2575-112">Double-click the **Title** in the navigation bar and update it to `New Event`.</span></span>
1. <span data-ttu-id="b2575-113">Usando a **biblioteca**, adicione um **item de** botão de barra ao lado esquerdo da barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="b2575-113">Using the **Library**, add a **Bar Button Item** to the left-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="b2575-114">Selecione o novo botão de barra e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="b2575-114">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="b2575-115">Altere **o título** para `Cancel` .</span><span class="sxs-lookup"><span data-stu-id="b2575-115">Change **Title** to `Cancel`.</span></span>
1. <span data-ttu-id="b2575-116">Usando a **biblioteca**, adicione um **item de** botão de barra ao lado direito da barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="b2575-116">Using the **Library**, add a **Bar Button Item** to the right-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="b2575-117">Selecione o novo botão de barra e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="b2575-117">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="b2575-118">Altere **o título** para `Create` .</span><span class="sxs-lookup"><span data-stu-id="b2575-118">Change **Title** to `Create`.</span></span>
1. <span data-ttu-id="b2575-119">Selecione o controlador de exibição e, em seguida, selecione o **Inspetor de Identidade.**</span><span class="sxs-lookup"><span data-stu-id="b2575-119">Select the view controller, then select the **Identity Inspector**.</span></span> <span data-ttu-id="b2575-120">Altere **a** classe **para NewEventViewController**.</span><span class="sxs-lookup"><span data-stu-id="b2575-120">Change **Class** to **NewEventViewController**.</span></span>
1. <span data-ttu-id="b2575-121">Adicione os seguintes controles da **Biblioteca** à exibição.</span><span class="sxs-lookup"><span data-stu-id="b2575-121">Add the following controls from the **Library** to the view.</span></span>

    - <span data-ttu-id="b2575-122">Adicione um **Rótulo** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="b2575-122">Add a **Label** under the navigation bar.</span></span> <span data-ttu-id="b2575-123">Definir seu texto como `Subject` .</span><span class="sxs-lookup"><span data-stu-id="b2575-123">Set its text to `Subject`.</span></span>
    - <span data-ttu-id="b2575-124">Adicione um **campo de** texto sob o rótulo.</span><span class="sxs-lookup"><span data-stu-id="b2575-124">Add a **Text Field** under the label.</span></span> <span data-ttu-id="b2575-125">De definir **seu atributo placeholder** como `Subject` .</span><span class="sxs-lookup"><span data-stu-id="b2575-125">Set its **Placeholder** attribute to `Subject`.</span></span>
    - <span data-ttu-id="b2575-126">Adicione um **Rótulo** sob o campo de texto.</span><span class="sxs-lookup"><span data-stu-id="b2575-126">Add a **Label** under the text field.</span></span> <span data-ttu-id="b2575-127">Definir seu texto como `Attendees` .</span><span class="sxs-lookup"><span data-stu-id="b2575-127">Set its text to `Attendees`.</span></span>
    - <span data-ttu-id="b2575-128">Adicione um **campo de** texto sob o rótulo.</span><span class="sxs-lookup"><span data-stu-id="b2575-128">Add a **Text Field** under the label.</span></span> <span data-ttu-id="b2575-129">De definir **seu atributo placeholder** como `Separate multiple entries with ;` .</span><span class="sxs-lookup"><span data-stu-id="b2575-129">Set its **Placeholder** attribute to `Separate multiple entries with ;`.</span></span>
    - <span data-ttu-id="b2575-130">Adicione um **Rótulo** sob o campo de texto.</span><span class="sxs-lookup"><span data-stu-id="b2575-130">Add a **Label** under the text field.</span></span> <span data-ttu-id="b2575-131">Definir seu texto como `Start` .</span><span class="sxs-lookup"><span data-stu-id="b2575-131">Set its text to `Start`.</span></span>
    - <span data-ttu-id="b2575-132">Adicione um **Selador de Data** sob o rótulo.</span><span class="sxs-lookup"><span data-stu-id="b2575-132">Add a **Date Picker** under the label.</span></span> <span data-ttu-id="b2575-133">Definir seu **estilo preferencial** para **compactar**, **seu intervalo** como **15 minutos** e sua altura como **35**.</span><span class="sxs-lookup"><span data-stu-id="b2575-133">Set its **Preferred Style** to **Compact**, its **Interval** to **15 minutes**, and its height to **35**.</span></span>
    - <span data-ttu-id="b2575-134">Adicione um **Rótulo** no selador de data.</span><span class="sxs-lookup"><span data-stu-id="b2575-134">Add a **Label** under the date picker.</span></span> <span data-ttu-id="b2575-135">Definir seu texto como `End` .</span><span class="sxs-lookup"><span data-stu-id="b2575-135">Set its text to `End`.</span></span>
    - <span data-ttu-id="b2575-136">Adicione um **Selador de Data** sob o rótulo.</span><span class="sxs-lookup"><span data-stu-id="b2575-136">Add a **Date Picker** under the label.</span></span> <span data-ttu-id="b2575-137">Definir seu **estilo preferencial** para **compactar**, **seu intervalo** como **15 minutos** e sua altura como **35**.</span><span class="sxs-lookup"><span data-stu-id="b2575-137">Set its **Preferred Style** to **Compact**, its **Interval** to **15 minutes**, and its height to **35**.</span></span>
    - <span data-ttu-id="b2575-138">Adicione um **ponto de exibição de** texto no selador de data.</span><span class="sxs-lookup"><span data-stu-id="b2575-138">Add a **Text View** under the date picker.</span></span>

1. <span data-ttu-id="b2575-139">Selecione o **Novo Controlador de Exibição de Evento** e use o Inspetor de **Conexão** para fazer as seguintes conexões.</span><span class="sxs-lookup"><span data-stu-id="b2575-139">Select the **New Event View Controller** and use the **Connection Inspector** to make the following connections.</span></span>

    - <span data-ttu-id="b2575-140">Conecte a **ação de** cancelamento recebida ao **botão Cancelar** barra.</span><span class="sxs-lookup"><span data-stu-id="b2575-140">Connect the **cancel** received action to the **Cancel** bar button.</span></span>
    - <span data-ttu-id="b2575-141">Conecte a **ação createEvent** recebida ao **botão Criar** barra.</span><span class="sxs-lookup"><span data-stu-id="b2575-141">Connect the **createEvent** received action to the **Create** bar button.</span></span>
    - <span data-ttu-id="b2575-142">Conecte a **saída do** assunto ao primeiro campo de texto.</span><span class="sxs-lookup"><span data-stu-id="b2575-142">Connect the **subject** outlet to the first text field.</span></span>
    - <span data-ttu-id="b2575-143">Conecte a **saída dos participantes** ao segundo campo de texto.</span><span class="sxs-lookup"><span data-stu-id="b2575-143">Connect the **attendees** outlet to the second text field.</span></span>
    - <span data-ttu-id="b2575-144">Conecte a **saída** de início ao primeiro se picker de data.</span><span class="sxs-lookup"><span data-stu-id="b2575-144">Connect the **start** outlet to the first date picker.</span></span>
    - <span data-ttu-id="b2575-145">Conecte a **saída final** ao segundo se picker de data.</span><span class="sxs-lookup"><span data-stu-id="b2575-145">Connect the **end** outlet to the second date picker.</span></span>
    - <span data-ttu-id="b2575-146">Conecte a **saída do** corpo ao exibição de texto.</span><span class="sxs-lookup"><span data-stu-id="b2575-146">Connect the **body** outlet to the text view.</span></span>

1. <span data-ttu-id="b2575-147">Adicione as restrições a seguir.</span><span class="sxs-lookup"><span data-stu-id="b2575-147">Add the following constraints.</span></span>

    - <span data-ttu-id="b2575-148">**Barra de Navegação**</span><span class="sxs-lookup"><span data-stu-id="b2575-148">**Navigation Bar**</span></span>
        - <span data-ttu-id="b2575-149">Espaço à frente para a Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-149">Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="b2575-150">Espaço à trailing para Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-150">Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="b2575-151">Espaço superior para Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-151">Top space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="b2575-152">Altura, valor: 44</span><span class="sxs-lookup"><span data-stu-id="b2575-152">Height, value: 44</span></span>
    - <span data-ttu-id="b2575-153">**Rótulo de Assunto**</span><span class="sxs-lookup"><span data-stu-id="b2575-153">**Subject Label**</span></span>
        - <span data-ttu-id="b2575-154">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-154">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-155">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-155">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-156">Espaço superior para a Barra de Navegação, valor: 20</span><span class="sxs-lookup"><span data-stu-id="b2575-156">Top space to Navigation Bar, value: 20</span></span>
    - <span data-ttu-id="b2575-157">**Campo Texto do Assunto**</span><span class="sxs-lookup"><span data-stu-id="b2575-157">**Subject Text Field**</span></span>
        - <span data-ttu-id="b2575-158">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-158">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-159">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-159">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-160">Espaço superior para o Rótulo de Assunto, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-160">Top space to Subject Label, value: Standard</span></span>
    - <span data-ttu-id="b2575-161">**Rótulo dos participantes**</span><span class="sxs-lookup"><span data-stu-id="b2575-161">**Attendees Label**</span></span>
        - <span data-ttu-id="b2575-162">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-162">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-163">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-163">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-164">Espaço superior para o Campo de Texto do Assunto, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-164">Top space to Subject Text Field, value: Standard</span></span>
    - <span data-ttu-id="b2575-165">**Campo de texto Participantes**</span><span class="sxs-lookup"><span data-stu-id="b2575-165">**Attendees Text Field**</span></span>
        - <span data-ttu-id="b2575-166">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-166">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-167">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-167">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-168">Espaço superior para o Rótulo de Participantes, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-168">Top space to Attendees Label, value: Standard</span></span>
    - <span data-ttu-id="b2575-169">**Rótulo iniciar**</span><span class="sxs-lookup"><span data-stu-id="b2575-169">**Start Label**</span></span>
        - <span data-ttu-id="b2575-170">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-170">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-171">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-171">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-172">Espaço superior para o Campo de Texto do Assunto, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-172">Top space to Subject Text Field, value: Standard</span></span>
    - <span data-ttu-id="b2575-173">**Start Date Picker**</span><span class="sxs-lookup"><span data-stu-id="b2575-173">**Start Date Picker**</span></span>
        - <span data-ttu-id="b2575-174">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-174">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-175">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-175">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-176">Espaço superior para o Rótulo de Participantes, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-176">Top space to Attendees Label, value: Standard</span></span>
        - <span data-ttu-id="b2575-177">Altura, valor: 35</span><span class="sxs-lookup"><span data-stu-id="b2575-177">Height, value: 35</span></span>
    - <span data-ttu-id="b2575-178">**Rótulo final**</span><span class="sxs-lookup"><span data-stu-id="b2575-178">**End Label**</span></span>
        - <span data-ttu-id="b2575-179">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-179">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-180">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-180">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-181">Espaço superior para o Se picker de data de início, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-181">Top space to Start Date Picker, value: Standard</span></span>
    - <span data-ttu-id="b2575-182">**End Date Picker**</span><span class="sxs-lookup"><span data-stu-id="b2575-182">**End Date Picker**</span></span>
        - <span data-ttu-id="b2575-183">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-183">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-184">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-184">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-185">Espaço superior para Rótulo Final, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-185">Top space to End Label, value: Standard</span></span>
        - <span data-ttu-id="b2575-186">Altura: 35</span><span class="sxs-lookup"><span data-stu-id="b2575-186">Height: 35</span></span>
    - <span data-ttu-id="b2575-187">**Exibição de Corpo de Texto**</span><span class="sxs-lookup"><span data-stu-id="b2575-187">**Body Text View**</span></span>
        - <span data-ttu-id="b2575-188">Espaço à esquerda para a margem exibir, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-188">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-189">Espaço à direita para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-189">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="b2575-190">Espaço superior para o Selador de Data de Término, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="b2575-190">Top space to End Date Picker, value: Standard</span></span>
        - <span data-ttu-id="b2575-191">Espaço inferior para Exibir margem, valor: 0</span><span class="sxs-lookup"><span data-stu-id="b2575-191">Bottom space to View margin, value: 0</span></span>

    ![Uma captura de tela do novo formulário de eventos no storyboard](images/new-event-form.png)

1. <span data-ttu-id="b2575-193">Selecione a **Cena do Calendário** e selecione o Inspetor de **Conexões.**</span><span class="sxs-lookup"><span data-stu-id="b2575-193">Select the **Calendar Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="b2575-194">Em **Segues Disparados,** arraste o círculo não preenchido ao lado de **manual** para o Novo Controlador de Modo de Exibição **de** Eventos no storyboard.</span><span class="sxs-lookup"><span data-stu-id="b2575-194">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **New Event View Controller** on the storyboard.</span></span> <span data-ttu-id="b2575-195">Selecione **Apresentar Modalmente** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="b2575-195">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="b2575-196">Selecione o segue que você acabou de adicionar e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="b2575-196">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="b2575-197">De definir **o campo Identificador** como `showEventForm` .</span><span class="sxs-lookup"><span data-stu-id="b2575-197">Set the **Identifier** field to `showEventForm`.</span></span>
1. <span data-ttu-id="b2575-198">Conecte a **ação showNewEventForm** recebida ao botão **+** da barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="b2575-198">Connect the **showNewEventForm** received action to the **+** navigation bar button.</span></span>
1. <span data-ttu-id="b2575-199">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b2575-199">Save your changes and restart the app.</span></span> <span data-ttu-id="b2575-200">Vá para a página de calendário e toque no **+** botão.</span><span class="sxs-lookup"><span data-stu-id="b2575-200">Go to the calendar page and tap the **+** button.</span></span> <span data-ttu-id="b2575-201">Preencha o formulário e toque em **Criar** para criar um novo evento.</span><span class="sxs-lookup"><span data-stu-id="b2575-201">Fill in the form and tap **Create** to create a new event.</span></span>

    ![Uma captura de tela do novo formulário de eventos](images/create-event.png)
