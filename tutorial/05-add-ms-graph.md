<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="06ebf-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06ebf-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="06ebf-102">Para este aplicativo, você usará o [SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="06ebf-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="06ebf-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="06ebf-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="06ebf-104">Nesta seção, você estenderá a `GraphManager` classe para adicionar uma função para obter os eventos e a atualização `CalendarViewController` do usuário para usar essas novas funções.</span><span class="sxs-lookup"><span data-stu-id="06ebf-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="06ebf-105">Abra **graphmanager. h** e adicione o seguinte código acima da `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="06ebf-105">Open **GraphManager.h** and add the following code above the `@interface` declaration.</span></span>

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSData* _Nullable data, NSError* _Nullable error);
    ```

1. <span data-ttu-id="06ebf-106">Adicione o seguinte código à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="06ebf-106">Add the following code to the `@interface` declaration.</span></span>

    ```objc
    - (void) getEventsWithCompletionBlock: (GetEventsCompletionBlock)completionBlock;
    ```

1. <span data-ttu-id="06ebf-107">Abra **graphmanager. m** e adicione a função a seguir à `GraphManager` classe.</span><span class="sxs-lookup"><span data-stu-id="06ebf-107">Open **GraphManager.m** and add the following function to the `GraphManager` class.</span></span>

    ```objc
    - (void) getEventsWithCompletionBlock:(GetEventsCompletionBlock)completionBlock {
        // GET /me/events?$select='subject,organizer,start,end'$orderby=createdDateTime DESC
        NSString* eventsUrlString =
        [NSString stringWithFormat:@"%@/me/events?%@&%@",
         MSGraphBaseURL,
         // Only return these fields in results
         @"$select=subject,organizer,start,end",
         // Sort results by when they were created, newest first
         @"$orderby=createdDateTime+DESC"];

        NSURL* eventsUrl = [[NSURL alloc] initWithString:eventsUrlString];
        NSMutableURLRequest* eventsRequest = [[NSMutableURLRequest alloc] initWithURL:eventsUrl];

        MSURLSessionDataTask* eventsDataTask =
        [[MSURLSessionDataTask alloc]
         initWithRequest:eventsRequest
         client:self.graphClient
         completion:^(NSData *data, NSURLResponse *response, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
                 return;
             }

             // TEMPORARY
             completionBlock(data, nil);
         }];

        // Execute the request
        [eventsDataTask execute];
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="06ebf-108">Considere o que o código `getEventsWithCompletionBlock` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="06ebf-108">Consider what the code in `getEventsWithCompletionBlock` is doing.</span></span>
    >
    > - <span data-ttu-id="06ebf-109">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="06ebf-109">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="06ebf-110">O `select` parâmetro de consulta limita os campos retornados para cada evento para apenas aqueles que o aplicativo usará realmente.</span><span class="sxs-lookup"><span data-stu-id="06ebf-110">The `select` query parameter limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="06ebf-111">O `orderby` parâmetro de consulta classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="06ebf-111">The `orderby` query parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="06ebf-112">Abra **CalendarViewController. m** e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06ebf-112">Open **CalendarViewController.m** and replace its entire contents with the following code.</span></span>

    ```objc
    #import "CalendarViewController.h"
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>

    @interface CalendarViewController ()

    @property SpinnerViewController* spinner;

    @end

    @implementation CalendarViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.

        self.spinner = [SpinnerViewController alloc];
        [self.spinner startWithContainer:self];

        [GraphManager.instance
         getEventsWithCompletionBlock:^(NSData * _Nullable data, NSError * _Nullable error) {
             dispatch_async(dispatch_get_main_queue(), ^{
                 [self.spinner stop];

                 if (error) {
                     // Show the error
                     UIAlertController* alert = [UIAlertController
                                                 alertControllerWithTitle:@"Error getting events"
                                                 message:error.debugDescription
                                                 preferredStyle:UIAlertControllerStyleAlert];

                     UIAlertAction* okButton = [UIAlertAction
                                                actionWithTitle:@"OK"
                                                style:UIAlertActionStyleDefault
                                                handler:nil];

                     [alert addAction:okButton];
                     [self presentViewController:alert animated:true completion:nil];
                     return;
                 }

                 // TEMPORARY
                 self.calendarJSON.text = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                 [self.calendarJSON sizeToFit];
             });
         }];
    }

    @end
    ```

1. <span data-ttu-id="06ebf-113">Execute o aplicativo, entre e toque no item de navegação de **calendário** no menu.</span><span class="sxs-lookup"><span data-stu-id="06ebf-113">Run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="06ebf-114">Você deve ver um despejo JSON dos eventos no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06ebf-114">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="06ebf-115">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="06ebf-115">Display the results</span></span>

<span data-ttu-id="06ebf-116">Agora você pode substituir o despejo JSON por algo para exibir os resultados de forma amigável.</span><span class="sxs-lookup"><span data-stu-id="06ebf-116">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="06ebf-117">Nesta seção, você irá modificar a `getEventsWithCompletionBlock` função para retornar objetos fortemente tipados e modificar `CalendarViewController` para usar um modo de exibição de tabela para renderizar os eventos.</span><span class="sxs-lookup"><span data-stu-id="06ebf-117">In this section, you will modify the `getEventsWithCompletionBlock` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

### <a name="update-getevents"></a><span data-ttu-id="06ebf-118">Atualizar GetEvents</span><span class="sxs-lookup"><span data-stu-id="06ebf-118">Update getEvents</span></span>

1. <span data-ttu-id="06ebf-119">Abra o **graphmanager. h**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-119">Open **GraphManager.h**.</span></span> <span data-ttu-id="06ebf-120">Altere a `GetEventsCompletionBlock` definição de tipo para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="06ebf-120">Change the `GetEventsCompletionBlock` type definition to the following.</span></span>

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSArray<MSGraphEvent*>* _Nullable events, NSError* _Nullable error);
    ```

1. <span data-ttu-id="06ebf-121">Abra o **graphmanager. m**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-121">Open **GraphManager.m**.</span></span> <span data-ttu-id="06ebf-122">Substitua a `completionBlock(data, nil);` linha na `getEventsWithCompletionBlock` função com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06ebf-122">Replace the `completionBlock(data, nil);` line in the `getEventsWithCompletionBlock` function with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphManager.m" id="GetEventsSnippet" highlight="24-43":::

### <a name="update-calendarviewcontroller"></a><span data-ttu-id="06ebf-123">Atualizar CalendarViewController</span><span class="sxs-lookup"><span data-stu-id="06ebf-123">Update CalendarViewController</span></span>

1. <span data-ttu-id="06ebf-124">Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** no projeto GraphTutorial `CalendarTableViewCell`chamado.</span><span class="sxs-lookup"><span data-stu-id="06ebf-124">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell`.</span></span> <span data-ttu-id="06ebf-125">Escolha **UITableViewCell** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="06ebf-125">Choose **UITableViewCell** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="06ebf-126">Abra **CalendarTableViewCell. h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06ebf-126">Open **CalendarTableViewCell.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.h" id="CalendarTableCellSnippet":::

1. <span data-ttu-id="06ebf-127">Abra **CalendarTableViewCell. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06ebf-127">Open **CalendarTableViewCell.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.m" id="CalendarTableCellSnippet":::

1. <span data-ttu-id="06ebf-128">Abra **Main. Storyboard** e localize a **cena do calendário**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-128">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="06ebf-129">Selecione o **modo de exibição** na **cena do calendário** e exclua-o.</span><span class="sxs-lookup"><span data-stu-id="06ebf-129">Select the **View** in the **Calendar Scene** and delete it.</span></span>

    ![Uma captura de tela do modo de exibição da cena de calendário](./images/view-in-calendar-scene.png)

1. <span data-ttu-id="06ebf-131">Adicione um **modo de exibição de tabela** da **biblioteca** à **cena do calendário**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-131">Add a **Table View** from the **Library** to the **Calendar Scene**.</span></span>
1. <span data-ttu-id="06ebf-132">Selecione o modo de exibição de tabela e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-132">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="06ebf-133">Definir **células de protótipo** como **1**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-133">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="06ebf-134">Use a **biblioteca** para adicionar três **Rótulos** à célula prototype.</span><span class="sxs-lookup"><span data-stu-id="06ebf-134">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="06ebf-135">Selecione a célula Prototype e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-135">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="06ebf-136">Altere a **classe** para **CalendarTableViewCell**.</span><span class="sxs-lookup"><span data-stu-id="06ebf-136">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="06ebf-137">Selecione o **Inspetor de atributos** e defina o `EventCell` **identificador** como.</span><span class="sxs-lookup"><span data-stu-id="06ebf-137">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="06ebf-138">Com o **EventCell** selecionado, selecione o **Inspetor de conexões** e `durationLabel`Conecte `organizerLabel`-se `subjectLabel` , e para os rótulos que você adicionou à célula no storyboard.</span><span class="sxs-lookup"><span data-stu-id="06ebf-138">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>
1. <span data-ttu-id="06ebf-139">Defina as propriedades e as restrições nos três rótulos da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="06ebf-139">Set the properties and constraints on the three labels as follows.</span></span>

    - <span data-ttu-id="06ebf-140">**Rótulo de assunto**</span><span class="sxs-lookup"><span data-stu-id="06ebf-140">**Subject Label**</span></span>
        - <span data-ttu-id="06ebf-141">Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-141">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-142">Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-142">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-143">Adicionar restrição: espaço superior à margem superior do modo de exibição de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-143">Add constraint: Top space to Content View Top Margin, value: 0</span></span>
    - <span data-ttu-id="06ebf-144">**Rótulo do organizador**</span><span class="sxs-lookup"><span data-stu-id="06ebf-144">**Organizer Label**</span></span>
        - <span data-ttu-id="06ebf-145">Fonte: System 12,0</span><span class="sxs-lookup"><span data-stu-id="06ebf-145">Font: System 12.0</span></span>
        - <span data-ttu-id="06ebf-146">Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-146">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-147">Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-147">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-148">Adicionar restrição: espaço superior para rótulo de assunto inferior, valor: padrão</span><span class="sxs-lookup"><span data-stu-id="06ebf-148">Add constraint: Top space to Subject Label Bottom, value: Standard</span></span>
    - <span data-ttu-id="06ebf-149">**Rótulo de duração**</span><span class="sxs-lookup"><span data-stu-id="06ebf-149">**Duration Label**</span></span>
        - <span data-ttu-id="06ebf-150">Fonte: System 12,0</span><span class="sxs-lookup"><span data-stu-id="06ebf-150">Font: System 12.0</span></span>
        - <span data-ttu-id="06ebf-151">Cor: cor cinza escuro</span><span class="sxs-lookup"><span data-stu-id="06ebf-151">Color: Dark Gray Color</span></span>
        - <span data-ttu-id="06ebf-152">Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-152">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-153">Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="06ebf-153">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="06ebf-154">Adicionar restrição: espaço superior ao rótulo do organizador inferior, valor: padrão</span><span class="sxs-lookup"><span data-stu-id="06ebf-154">Add constraint: Top space to Organizer Label Bottom, value: Standard</span></span>
        - <span data-ttu-id="06ebf-155">Adicionar restrição: espaço inferior à margem inferior da visualização de conteúdo, valor: 8</span><span class="sxs-lookup"><span data-stu-id="06ebf-155">Add constraint: Bottom space to Content View Bottom Margin, value: 8</span></span>

    ![Uma captura de tela do layout da célula de protótipo](./images/prototype-cell-layout.png)

1. <span data-ttu-id="06ebf-157">Abra **CalendarViewController. h** e remova a `calendarJSON` propriedade.</span><span class="sxs-lookup"><span data-stu-id="06ebf-157">Open **CalendarViewController.h** and remove the `calendarJSON` property.</span></span>
1. <span data-ttu-id="06ebf-158">Altere a `@interface` declaração para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="06ebf-158">Change the `@interface` declaration to the following.</span></span>

    ```objc
    @interface CalendarViewController : UITableViewController
    ```

1. <span data-ttu-id="06ebf-159">Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06ebf-159">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.m" id="CalendarViewSnippet":::

1. <span data-ttu-id="06ebf-160">Execute o aplicativo, entre e toque na guia **calendário** . Você deve ver a lista de eventos.</span><span class="sxs-lookup"><span data-stu-id="06ebf-160">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/calendar-list.png)
