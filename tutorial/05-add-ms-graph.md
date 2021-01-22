<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ffece-101">Neste exercício, você incorporará o Microsoft Graph ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ffece-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="ffece-102">Para este aplicativo, você usará o [SDK](https://github.com/microsoftgraph/msgraph-sdk-objc) do Microsoft Graph para o Objetivo C para fazer chamadas ao Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ffece-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="ffece-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="ffece-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="ffece-104">Nesta seção, você estenderá a classe para adicionar uma função para obter os eventos do usuário para a semana atual e atualizar para `GraphManager` `CalendarViewController` usar essa nova função.</span><span class="sxs-lookup"><span data-stu-id="ffece-104">In this section you will extend the `GraphManager` class to add a function to get the user's events for the current week and update `CalendarViewController` to use this new function.</span></span>

1. <span data-ttu-id="ffece-105">Abra **GraphManager.h** e adicione o seguinte código acima da `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="ffece-105">Open **GraphManager.h** and add the following code above the `@interface` declaration.</span></span>

    ```objc
    typedef void (^GetCalendarViewCompletionBlock)(NSData* _Nullable data,
                                                   NSError* _Nullable error);
    ```

1. <span data-ttu-id="ffece-106">Adicione o código a seguir à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="ffece-106">Add the following code to the `@interface` declaration.</span></span>

    ```objc
    - (void) getCalendarViewStartingAt: (NSString*) viewStart
                              endingAt: (NSString*) viewEnd
                   withCompletionBlock: (GetCalendarViewCompletionBlock) completion;
    ```

1. <span data-ttu-id="ffece-107">Abra **GraphManager.m** e adicione a função a seguir à `GraphManager` classe.</span><span class="sxs-lookup"><span data-stu-id="ffece-107">Open **GraphManager.m** and add the following function to the `GraphManager` class.</span></span>

    ```objc
    - (void) getCalendarViewStartingAt: (NSString *) viewStart
                              endingAt: (NSString *) viewEnd
                   withCompletionBlock: (GetCalendarViewCompletionBlock) completion {
        // Set calendar view start and end parameters
        NSString* viewStartEndString =
        [NSString stringWithFormat:@"startDateTime=%@&endDateTime=%@",
         viewStart,
         viewEnd];

        // GET /me/calendarview
        NSString* eventsUrlString =
        [NSString stringWithFormat:@"%@/me/calendarview?%@&%@&%@&%@",
         MSGraphBaseURL,
         viewStartEndString,
         // Only return these fields in results
         @"$select=subject,organizer,start,end",
         // Sort results by start time
         @"$orderby=start/dateTime",
         // Request at most 25 results
         @"$top=25"];

        NSURL* eventsUrl = [[NSURL alloc] initWithString:eventsUrlString];
        NSMutableURLRequest* eventsRequest = [[NSMutableURLRequest alloc] initWithURL:eventsUrl];

        // Add the Prefer: outlook.timezone header to get start and end times
        // in user's time zone
        NSString* preferHeader =
        [NSString stringWithFormat:@"outlook.timezone=\"%@\"",
         self.graphTimeZone];
        [eventsRequest addValue:preferHeader forHTTPHeaderField:@"Prefer"];

        MSURLSessionDataTask* eventsDataTask =
        [[MSURLSessionDataTask alloc]
         initWithRequest:eventsRequest
         client:self.graphClient
         completion:^(NSData *data, NSURLResponse *response, NSError *error) {
             if (error) {
                 completion(nil, error);
                 return;
             }

             // TEMPORARY
             completion(data, nil);
         }];

        // Execute the request
        [eventsDataTask execute];
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="ffece-108">Considere o que o código `getCalendarViewStartingAt` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="ffece-108">Consider what the code in `getCalendarViewStartingAt` is doing.</span></span>
    >
    > - <span data-ttu-id="ffece-109">A URL que será chamada é `/v1.0/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="ffece-109">The URL that will be called is `/v1.0/me/calendarview`.</span></span>
    >   - <span data-ttu-id="ffece-110">Os `startDateTime` `endDateTime` parâmetros e consulta definem o início e o fim da exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="ffece-110">The `startDateTime` and `endDateTime` query parameters define the start and end of the calendar view.</span></span>
    >   - <span data-ttu-id="ffece-111">O `select` parâmetro de consulta limita os campos retornados para cada evento a apenas aqueles que o ponto de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="ffece-111">The `select` query parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    >   - <span data-ttu-id="ffece-112">O `orderby` parâmetro de consulta classifica os resultados por hora de início.</span><span class="sxs-lookup"><span data-stu-id="ffece-112">The `orderby` query parameter sorts the results by start time.</span></span>
    >   - <span data-ttu-id="ffece-113">O `top` parâmetro de consulta solicita 25 resultados por página.</span><span class="sxs-lookup"><span data-stu-id="ffece-113">The `top` query parameter requests 25 results per page.</span></span>
    >   - <span data-ttu-id="ffece-114">o header faz com que o Microsoft Graph retorne as horas de início e término de cada evento `Prefer: outlook.timezone` no fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="ffece-114">the `Prefer: outlook.timezone` header causes the Microsoft Graph to return the start and end times of each event in the user's time zone.</span></span>

1. <span data-ttu-id="ffece-115">Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **GraphToIana**.</span><span class="sxs-lookup"><span data-stu-id="ffece-115">Create a new **Cocoa Touch Class** in the **GraphTutorial** project named **GraphToIana**.</span></span> <span data-ttu-id="ffece-116">Escolha **NSObject** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="ffece-116">Choose **NSObject** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="ffece-117">Abra **GraphToIana.h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-117">Open **GraphToIana.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.h" id="GraphToIanaSnippet":::

1. <span data-ttu-id="ffece-118">Abra **GraphToIana.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-118">Open **GraphToIana.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.m" id="GraphToIanaSnippet":::

    <span data-ttu-id="ffece-119">Isso faz uma pesquisa simples para encontrar um identificador de fuso horário IANA com base no nome do fuso horário retornado pelo Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ffece-119">This does a simple lookup to find an IANA time zone identifier based on the time zone name returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="ffece-120">Abra **CalendarViewController.m e** substitua seu conteúdo inteiro pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-120">Open **CalendarViewController.m** and replace its entire contents with the following code.</span></span>

    ```objc
    #import "CalendarViewController.h"
    #import "SpinnerViewController.h"
    #import "GraphManager.h"
    #import "GraphToIana.h"
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

        // Calculate the start and end of the current week
        NSString* timeZoneId = [GraphToIana
                                getIanaIdentifierFromGraphIdentifier:
                                [GraphManager.instance graphTimeZone]];

        NSDate* now = [NSDate date];
        NSCalendar* calendar = [NSCalendar calendarWithIdentifier:NSCalendarIdentifierGregorian];
        NSTimeZone* timeZone = [NSTimeZone timeZoneWithName:timeZoneId];
        [calendar setTimeZone:timeZone];

        NSDateComponents* startOfWeekComponents = [calendar
                                                   components:NSCalendarUnitCalendar |
                                                   NSCalendarUnitYearForWeekOfYear |
                                                   NSCalendarUnitWeekOfYear
                                                   fromDate:now];
        NSDate* startOfWeek = [startOfWeekComponents date];
        NSDate* endOfWeek = [calendar dateByAddingUnit:NSCalendarUnitDay
                                                 value:7
                                                toDate:startOfWeek
                                               options:0];

        // Convert start and end to ISO 8601 strings
        NSISO8601DateFormatter* isoFormatter = [[NSISO8601DateFormatter alloc] init];
        NSString* viewStart = [isoFormatter stringFromDate:startOfWeek];
        NSString* viewEnd = [isoFormatter stringFromDate:endOfWeek];

        [GraphManager.instance
         getCalendarViewStartingAt:viewStart
         endingAt:viewEnd
         withCompletionBlock:^(NSData * _Nullable data, NSError * _Nullable error) {
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

1. <span data-ttu-id="ffece-121">Execute o aplicativo, entre e toque no item **de** navegação calendário no menu.</span><span class="sxs-lookup"><span data-stu-id="ffece-121">Run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="ffece-122">Você deverá ver um despejo JSON dos eventos no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ffece-122">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="ffece-123">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="ffece-123">Display the results</span></span>

<span data-ttu-id="ffece-124">Agora você pode substituir o despejo JSON por algo para exibir os resultados de maneira amigável.</span><span class="sxs-lookup"><span data-stu-id="ffece-124">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="ffece-125">Nesta seção, você modificará a função para retornar objetos fortemente digitados e modificará para usar um exibição de tabela para `getCalendarViewStartingAt` `CalendarViewController` renderizar os eventos.</span><span class="sxs-lookup"><span data-stu-id="ffece-125">In this section, you will modify the `getCalendarViewStartingAt` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

### <a name="update-getcalendarviewstartingat"></a><span data-ttu-id="ffece-126">Atualizar getCalendarViewStartingAt</span><span class="sxs-lookup"><span data-stu-id="ffece-126">Update getCalendarViewStartingAt</span></span>

1. <span data-ttu-id="ffece-127">Abra **GraphManager.h.**</span><span class="sxs-lookup"><span data-stu-id="ffece-127">Open **GraphManager.h**.</span></span> <span data-ttu-id="ffece-128">Altere `GetCalendarViewCompletionBlock` a definição de tipo para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="ffece-128">Change the `GetCalendarViewCompletionBlock` type definition to the following.</span></span>

    ```objc
    typedef void (^GetCalendarViewCompletionBlock)(NSArray<MSGraphEvent*>* _Nullable events, NSError* _Nullable error);
    ```

1. <span data-ttu-id="ffece-129">Abra **GraphManager.m.**</span><span class="sxs-lookup"><span data-stu-id="ffece-129">Open **GraphManager.m**.</span></span> <span data-ttu-id="ffece-130">Substitua a função `getCalendarViewStartingAt` existente pelo código seguinte.</span><span class="sxs-lookup"><span data-stu-id="ffece-130">Replace the existing `getCalendarViewStartingAt` function with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphManager.m" id="GetCalendarViewSnippet" highlight="42-61":::

### <a name="update-calendarviewcontroller"></a><span data-ttu-id="ffece-131">Atualizar CalendarViewController</span><span class="sxs-lookup"><span data-stu-id="ffece-131">Update CalendarViewController</span></span>

1. <span data-ttu-id="ffece-132">Crie um novo **arquivo de classe Cocoa Touch** no projeto **GraphTutorial** chamado `CalendarTableViewCell` .</span><span class="sxs-lookup"><span data-stu-id="ffece-132">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell`.</span></span> <span data-ttu-id="ffece-133">Escolha **UITableViewCell** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="ffece-133">Choose **UITableViewCell** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="ffece-134">Abra **CalendarTableViewCell.h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-134">Open **CalendarTableViewCell.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.h" id="CalendarTableCellSnippet":::

1. <span data-ttu-id="ffece-135">Abra **CalendarTableViewCell.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-135">Open **CalendarTableViewCell.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.m" id="CalendarTableCellSnippet":::

1. <span data-ttu-id="ffece-136">Crie um novo **arquivo de classe Cocoa Touch** no projeto **GraphTutorial** chamado `CalendarTableViewController` .</span><span class="sxs-lookup"><span data-stu-id="ffece-136">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewController`.</span></span> <span data-ttu-id="ffece-137">Escolha **UITableViewController** na **Subclasse do** campo.</span><span class="sxs-lookup"><span data-stu-id="ffece-137">Choose **UITableViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="ffece-138">Abra **CalendarTableViewController.h e** substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-138">Open **CalendarTableViewController.h** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.h" id="CalendarTableViewControllerSnippet":::

1. <span data-ttu-id="ffece-139">Abra **CalendarTableViewController.m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-139">Open **CalendarTableViewController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.m" id="CalendarTableViewControllerSnippet":::

1. <span data-ttu-id="ffece-140">Abra **Main.storyboard** e localize a **cena do calendário.**</span><span class="sxs-lookup"><span data-stu-id="ffece-140">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="ffece-141">Exclua o visualizador de rolagem do visualização raiz.</span><span class="sxs-lookup"><span data-stu-id="ffece-141">Delete the scroll view from the root view.</span></span>
1. <span data-ttu-id="ffece-142">Usando a **Biblioteca,** adicione uma **Barra de Navegação** à parte superior do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="ffece-142">Using the **Library**, add a **Navigation Bar** to the top of the view.</span></span>
1. <span data-ttu-id="ffece-143">Clique duas vezes no **título** na barra de navegação e atualize-o para `Calendar` .</span><span class="sxs-lookup"><span data-stu-id="ffece-143">Double-click the **Title** in the navigation bar and update it to `Calendar`.</span></span>
1. <span data-ttu-id="ffece-144">Usando a **biblioteca**, adicione um **item de** botão de barra ao lado direito da barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="ffece-144">Using the **Library**, add a **Bar Button Item** to the right-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="ffece-145">Selecione o novo botão de barra e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="ffece-145">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="ffece-146">Alterar **Imagem para** **mais**.</span><span class="sxs-lookup"><span data-stu-id="ffece-146">Change **Image** to **plus**.</span></span>
1. <span data-ttu-id="ffece-147">Adicione um **modo de exibição** de contêiner **da Biblioteca** ao modo de exibição na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="ffece-147">Add a **Container View** from the **Library** to the view under the navigation bar.</span></span> <span data-ttu-id="ffece-148">Reize a exibição do contêiner para que pegue todo o espaço restante no ponto de vista.</span><span class="sxs-lookup"><span data-stu-id="ffece-148">Resize the container view to take all of the remaining space in the view.</span></span>
1. <span data-ttu-id="ffece-149">Definir restrições na barra de navegação e no modo de exibição de contêiner da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="ffece-149">Set constraints on the navigation bar and container view as follows.</span></span>

    - <span data-ttu-id="ffece-150">**Barra de Navegação**</span><span class="sxs-lookup"><span data-stu-id="ffece-150">**Navigation Bar**</span></span>
        - <span data-ttu-id="ffece-151">Adicionar restrição: Altura, valor: 44</span><span class="sxs-lookup"><span data-stu-id="ffece-151">Add constraint: Height, value: 44</span></span>
        - <span data-ttu-id="ffece-152">Restrição Adicionar: Espaço à frente da Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-152">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="ffece-153">Adicionar restrição: Espaço à trailing para a Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-153">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="ffece-154">Restrição Adicionar: Espaço superior à Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-154">Add constraint: Top space to Safe Area, value: 0</span></span>
    - <span data-ttu-id="ffece-155">**Exibição de Contêiner**</span><span class="sxs-lookup"><span data-stu-id="ffece-155">**Container View**</span></span>
        - <span data-ttu-id="ffece-156">Restrição Adicionar: Espaço à frente da Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-156">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="ffece-157">Adicionar restrição: Espaço à trailing para a Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-157">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="ffece-158">Restrição Adicionar: Espaço superior à Barra de Navegação Inferior, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-158">Add constraint: Top space to Navigation Bar Bottom, value: 0</span></span>
        - <span data-ttu-id="ffece-159">Adicionar restrição: Espaço inferior à Área de Segurança, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-159">Add constraint: Bottom space to Safe Area, value: 0</span></span>

1. <span data-ttu-id="ffece-160">Localize o segundo controlador de exibição adicionado ao storyboard quando você adicionou o ponto de vista do contêiner.</span><span class="sxs-lookup"><span data-stu-id="ffece-160">Locate the second view controller added to the storyboard when you added the container view.</span></span> <span data-ttu-id="ffece-161">Ele é conectado à **cena do calendário** por um segue de insusuidade.</span><span class="sxs-lookup"><span data-stu-id="ffece-161">It is connected to the **Calendar Scene** by an embed segue.</span></span> <span data-ttu-id="ffece-162">Selecione este controlador e use o **Inspetor de Identidade** para alterar a **classe** para **CalendarTableViewController**.</span><span class="sxs-lookup"><span data-stu-id="ffece-162">Select this controller and use the **Identity Inspector** to change **Class** to **CalendarTableViewController**.</span></span>
1. <span data-ttu-id="ffece-163">Exclua **o view** do controlador **de exibição de tabela de calendário.**</span><span class="sxs-lookup"><span data-stu-id="ffece-163">Delete the **View** from the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="ffece-164">Adicione um **exibição de tabela** da **biblioteca ao** controlador **de exibição de tabela de calendário.**</span><span class="sxs-lookup"><span data-stu-id="ffece-164">Add a **Table View** from the **Library** to the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="ffece-165">Selecione o exibição de tabela e selecione o **Inspetor de Atributos.**</span><span class="sxs-lookup"><span data-stu-id="ffece-165">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="ffece-166">Definir **células de protótipo** como **1**.</span><span class="sxs-lookup"><span data-stu-id="ffece-166">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="ffece-167">Arraste a borda inferior da célula de protótipo para lhe dar uma área maior para trabalhar.</span><span class="sxs-lookup"><span data-stu-id="ffece-167">Drag the bottom edge of the prototype cell to give you a larger area to work with.</span></span>
1. <span data-ttu-id="ffece-168">Use a **Biblioteca para** adicionar três **Rótulos** à célula de protótipo.</span><span class="sxs-lookup"><span data-stu-id="ffece-168">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="ffece-169">Selecione a célula de protótipo e, em seguida, selecione o **Inspetor de Identidade.**</span><span class="sxs-lookup"><span data-stu-id="ffece-169">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="ffece-170">Alterar **classe** para **CalendarTableViewCell**.</span><span class="sxs-lookup"><span data-stu-id="ffece-170">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="ffece-171">Selecione o **Inspetor de Atributos** e de definir o **identificador** como `EventCell` .</span><span class="sxs-lookup"><span data-stu-id="ffece-171">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="ffece-172">Com o **EventCell** selecionado, selecione o Inspetor de Conexões e conecte-se, e aos rótulos que você adicionou à célula no  `durationLabel` `organizerLabel` `subjectLabel` storyboard.</span><span class="sxs-lookup"><span data-stu-id="ffece-172">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>
1. <span data-ttu-id="ffece-173">De definidas as propriedades e restrições nos três rótulos da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="ffece-173">Set the properties and constraints on the three labels as follows.</span></span>

    - <span data-ttu-id="ffece-174">**Rótulo de Assunto**</span><span class="sxs-lookup"><span data-stu-id="ffece-174">**Subject Label**</span></span>
        - <span data-ttu-id="ffece-175">Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-175">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-176">Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-176">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-177">Adicionar restrição: Espaço superior à Margem Superior do Exibição de Conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-177">Add constraint: Top space to Content View Top Margin, value: 0</span></span>
    - <span data-ttu-id="ffece-178">**Rótulo do Organizador**</span><span class="sxs-lookup"><span data-stu-id="ffece-178">**Organizer Label**</span></span>
        - <span data-ttu-id="ffece-179">Fonte: Sistema 12.0</span><span class="sxs-lookup"><span data-stu-id="ffece-179">Font: System 12.0</span></span>
        - <span data-ttu-id="ffece-180">Restrição Adicionar: Altura, valor: 15</span><span class="sxs-lookup"><span data-stu-id="ffece-180">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="ffece-181">Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-181">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-182">Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-182">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-183">Adicionar restrição: Espaço superior para o Rótulo de Assunto Inferior, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="ffece-183">Add constraint: Top space to Subject Label Bottom, value: Standard</span></span>
    - <span data-ttu-id="ffece-184">**Rótulo de Duração**</span><span class="sxs-lookup"><span data-stu-id="ffece-184">**Duration Label**</span></span>
        - <span data-ttu-id="ffece-185">Fonte: Sistema 12.0</span><span class="sxs-lookup"><span data-stu-id="ffece-185">Font: System 12.0</span></span>
        - <span data-ttu-id="ffece-186">Cor: Cor Cinza Escuro</span><span class="sxs-lookup"><span data-stu-id="ffece-186">Color: Dark Gray Color</span></span>
        - <span data-ttu-id="ffece-187">Restrição Adicionar: Altura, valor: 15</span><span class="sxs-lookup"><span data-stu-id="ffece-187">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="ffece-188">Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-188">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-189">Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-189">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="ffece-190">Restrição Adicionar: Espaço superior para o Rótulo do Organizador Inferior, valor: Padrão</span><span class="sxs-lookup"><span data-stu-id="ffece-190">Add constraint: Top space to Organizer Label Bottom, value: Standard</span></span>
        - <span data-ttu-id="ffece-191">Adicionar restrição: Espaço inferior à Margem Inferior do Exibição de Conteúdo, valor: 0</span><span class="sxs-lookup"><span data-stu-id="ffece-191">Add constraint: Bottom space to Content View Bottom Margin, value: 0</span></span>

1. <span data-ttu-id="ffece-192">Select the **EventCell**, then select the **Size Inspector**.</span><span class="sxs-lookup"><span data-stu-id="ffece-192">Select the **EventCell**, then select the **Size Inspector**.</span></span> <span data-ttu-id="ffece-193">**Habilitar Automático** para **Altura da Linha.**</span><span class="sxs-lookup"><span data-stu-id="ffece-193">Enable **Automatic** for **Row Height**.</span></span>

    ![Uma captura de tela dos controladores de exibição de calendário e tabela de calendário](images/calendar-table-storyboard.png)

1. <span data-ttu-id="ffece-195">Abra **CalendarViewController.h** e remova a `calendarJSON` propriedade.</span><span class="sxs-lookup"><span data-stu-id="ffece-195">Open **CalendarViewController.h** and remove the `calendarJSON` property.</span></span>

1. <span data-ttu-id="ffece-196">Abra **CalendarViewController.m e** substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ffece-196">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.m" id="CalendarViewControllerSnippet":::

1. <span data-ttu-id="ffece-197">Execute o aplicativo, entre e toque na **guia** Calendário. Você deverá ver a lista de eventos.</span><span class="sxs-lookup"><span data-stu-id="ffece-197">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/calendar-list.png)
