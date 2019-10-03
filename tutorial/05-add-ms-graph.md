<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5634c-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5634c-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="5634c-102">Para este aplicativo, você usará o [SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5634c-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="5634c-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="5634c-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="5634c-104">Nesta seção, você estenderá a `GraphManager` classe para adicionar uma função para obter os eventos e a atualização `CalendarViewController` do usuário para usar essas novas funções.</span><span class="sxs-lookup"><span data-stu-id="5634c-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="5634c-105">Abra **graphmanager. h** e adicione o seguinte código acima da `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="5634c-105">Open **GraphManager.h** and add the following code above the `@interface` declaration.</span></span>

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSData* _Nullable data, NSError* _Nullable error);
    ```

1. <span data-ttu-id="5634c-106">Adicione o seguinte código à `@interface` declaração.</span><span class="sxs-lookup"><span data-stu-id="5634c-106">Add the following code to the `@interface` declaration.</span></span>

    ```objc
    - (void) getEventsWithCompletionBlock: (GetEventsCompletionBlock)completionBlock;
    ```

1. <span data-ttu-id="5634c-107">Abra **graphmanager. m** e adicione a função a seguir à `GraphManager` classe.</span><span class="sxs-lookup"><span data-stu-id="5634c-107">Open **GraphManager.m** and add the following function to the `GraphManager` class.</span></span>

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
    > <span data-ttu-id="5634c-108">Considere o que o código `getEventsWithCompletionBlock` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="5634c-108">Consider what the code in `getEventsWithCompletionBlock` is doing.</span></span>
    >
    > - <span data-ttu-id="5634c-109">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="5634c-109">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="5634c-110">O `select` parâmetro de consulta limita os campos retornados para cada evento para apenas aqueles que o aplicativo usará realmente.</span><span class="sxs-lookup"><span data-stu-id="5634c-110">The `select` query parameter limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="5634c-111">O `orderby` parâmetro de consulta classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="5634c-111">The `orderby` query parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="5634c-112">Abra **CalendarViewController. m** e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5634c-112">Open **CalendarViewController.m** and replace its entire contents with the following code.</span></span>

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

<span data-ttu-id="5634c-113">Agora você pode executar o aplicativo, entrar e tocar no item de navegação de **calendário** no menu.</span><span class="sxs-lookup"><span data-stu-id="5634c-113">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="5634c-114">Você deve ver um despejo JSON dos eventos no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5634c-114">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="5634c-115">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="5634c-115">Display the results</span></span>

<span data-ttu-id="5634c-116">Agora você pode substituir o despejo JSON por algo para exibir os resultados de forma amigável.</span><span class="sxs-lookup"><span data-stu-id="5634c-116">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="5634c-117">Nesta seção, você irá modificar a `getEventsWithCompletionBlock` função para retornar objetos fortemente tipados e modificar `CalendarViewController` para usar um modo de exibição de tabela para renderizar os eventos.</span><span class="sxs-lookup"><span data-stu-id="5634c-117">In this section, you will modify the `getEventsWithCompletionBlock` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

### <a name="update-getevents"></a><span data-ttu-id="5634c-118">Atualizar GetEvents</span><span class="sxs-lookup"><span data-stu-id="5634c-118">Update getEvents</span></span>

1. <span data-ttu-id="5634c-119">Abra o **graphmanager. h**.</span><span class="sxs-lookup"><span data-stu-id="5634c-119">Open **GraphManager.h**.</span></span> <span data-ttu-id="5634c-120">Altere a `GetEventsCompletionBlock` definição de tipo para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="5634c-120">Change the `GetEventsCompletionBlock` type definition to the following.</span></span>

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSArray<MSGraphEvent*>* _Nullable events, NSError* _Nullable error);
    ```

1. <span data-ttu-id="5634c-121">Abra o **graphmanager. m**.</span><span class="sxs-lookup"><span data-stu-id="5634c-121">Open **GraphManager.m**.</span></span> <span data-ttu-id="5634c-122">Substitua a `completionBlock(data, nil);` linha na `getEventsWithCompletionBlock` função com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5634c-122">Replace the `completionBlock(data, nil);` line in the `getEventsWithCompletionBlock` function with the following code.</span></span>

    ```objc
    NSError* graphError;

    // Deserialize to an events collection
    MSCollection* eventsCollection = [[MSCollection alloc] initWithData:data error:&graphError];
    if (graphError) {
        completionBlock(nil, graphError);
        return;
    }

    // Create an array to return
    NSMutableArray* eventsArray = [[NSMutableArray alloc]
                                initWithCapacity:eventsCollection.value.count];

    for (id event in eventsCollection.value) {
        // Deserialize the event and add to the array
        MSGraphEvent* graphEvent = [[MSGraphEvent alloc] initWithDictionary:event];
        [eventsArray addObject:graphEvent];
    }

    completionBlock(eventsArray, nil);
    ```

### <a name="update-calendarviewcontroller"></a><span data-ttu-id="5634c-123">Atualizar CalendarViewController</span><span class="sxs-lookup"><span data-stu-id="5634c-123">Update CalendarViewController</span></span>

1. <span data-ttu-id="5634c-124">Crie um novo arquivo de **classe Touch** do Cocoa \*\*\*\* no projeto GraphTutorial `CalendarTableViewCell`chamado.</span><span class="sxs-lookup"><span data-stu-id="5634c-124">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell`.</span></span> <span data-ttu-id="5634c-125">Escolha **UITableViewCell** na **subclasse de** Field.</span><span class="sxs-lookup"><span data-stu-id="5634c-125">Choose **UITableViewCell** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="5634c-126">Abra **CalendarTableViewCell. h** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5634c-126">Open **CalendarTableViewCell.h** and replace its contents with the following code.</span></span>

    ```objc
    #import <UIKit/UIKit.h>

    NS_ASSUME_NONNULL_BEGIN

    @interface CalendarTableViewCell : UITableViewCell

    @property (nonatomic) NSString* subject;
    @property (nonatomic) NSString* organizer;
    @property (nonatomic) NSString* duration;

    @end

    NS_ASSUME_NONNULL_END
    ```

1. <span data-ttu-id="5634c-127">Abra **CalendarTableViewCell. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5634c-127">Open **CalendarTableViewCell.m** and replace its contents with the following code.</span></span>

    ```objc
    #import "CalendarTableViewCell.h"

    @interface CalendarTableViewCell()

    @property (nonatomic) IBOutlet UILabel *subjectLabel;
    @property (nonatomic) IBOutlet UILabel *organizerLabel;
    @property (nonatomic) IBOutlet UILabel *durationLabel;

    @end

    @implementation CalendarTableViewCell

    - (void)awakeFromNib {
        [super awakeFromNib];
        // Initialization code
    }

    - (void)setSelected:(BOOL)selected animated:(BOOL)animated {
        [super setSelected:selected animated:animated];

        // Configure the view for the selected state
    }

    - (void) setSubject:(NSString *)subject {
        _subject = subject;
        self.subjectLabel.text = subject;
        [self.subjectLabel sizeToFit];
    }

    - (void) setOrganizer:(NSString *)organizer {
        _organizer = organizer;
        self.organizerLabel.text = organizer;
        [self.organizerLabel sizeToFit];
    }

    - (void) setDuration:(NSString *)duration {
        _duration = duration;
        self.durationLabel.text = duration;
        [self.durationLabel sizeToFit];
    }

    @end
    ```

1. <span data-ttu-id="5634c-128">Abra **Main. Storyboard** e localize a **cena do calendário**.</span><span class="sxs-lookup"><span data-stu-id="5634c-128">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="5634c-129">Selecione o **modo de exibição** na **cena do calendário** e exclua-o.</span><span class="sxs-lookup"><span data-stu-id="5634c-129">Select the **View** in the **Calendar Scene** and delete it.</span></span>

    ![Uma captura de tela do modo de exibição da cena de calendário](./images/view-in-calendar-scene.png)

1. <span data-ttu-id="5634c-131">Adicione um **modo de exibição de tabela** da **biblioteca** à **cena do calendário**.</span><span class="sxs-lookup"><span data-stu-id="5634c-131">Add a **Table View** from the **Library** to the **Calendar Scene**.</span></span>
1. <span data-ttu-id="5634c-132">Selecione o modo de exibição de tabela e, em seguida, selecione o **Inspetor de atributos**.</span><span class="sxs-lookup"><span data-stu-id="5634c-132">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="5634c-133">Definir **células de protótipo** como **1**.</span><span class="sxs-lookup"><span data-stu-id="5634c-133">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="5634c-134">Use a **biblioteca** para adicionar três **Rótulos** à célula prototype.</span><span class="sxs-lookup"><span data-stu-id="5634c-134">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="5634c-135">Selecione a célula Prototype e, em seguida, selecione o **Inspetor de identidade**.</span><span class="sxs-lookup"><span data-stu-id="5634c-135">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="5634c-136">Altere a **classe** para **CalendarTableViewCell**.</span><span class="sxs-lookup"><span data-stu-id="5634c-136">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="5634c-137">Selecione o **Inspetor de atributos** e defina o `EventCell` **identificador** como.</span><span class="sxs-lookup"><span data-stu-id="5634c-137">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="5634c-138">No menu **Editor** , selecione **resolver problemas de layout automático**e, em seguida, selecione **Adicionar restrições ausentes** abaixo **de todos os modos de exibição no controlador de exibição de boas-vindas**</span><span class="sxs-lookup"><span data-stu-id="5634c-138">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>
1. <span data-ttu-id="5634c-139">Com o **EventCell** selecionado, selecione o **Inspetor de conexões** e `durationLabel`Conecte `organizerLabel`-se `subjectLabel` , e para os rótulos que você adicionou à célula no storyboard.</span><span class="sxs-lookup"><span data-stu-id="5634c-139">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>

    ![Uma captura de tela do layout da célula de protótipo](./images/prototype-cell-layout.png)

1. <span data-ttu-id="5634c-141">Abra **CalendarViewController. h** e remova a `calendarJSON` propriedade.</span><span class="sxs-lookup"><span data-stu-id="5634c-141">Open **CalendarViewController.h** and remove the `calendarJSON` property.</span></span>
1. <span data-ttu-id="5634c-142">Altere a `@interface` declaração para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="5634c-142">Change the `@interface` declaration to the following.</span></span>

    ```objc
    @interface CalendarViewController : UITableViewController
    ```

1. <span data-ttu-id="5634c-143">Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5634c-143">Open **CalendarViewController.m** and replace its contents with the following code.</span></span>

    ```objc
    #import "WelcomeViewController.h"
    #import "SpinnerViewController.h"
    #import "AuthenticationManager.h"
    #import "GraphManager.h"
    #import <MSGraphClientModels/MSGraphClientModels.h>

    @interface WelcomeViewController ()

    @property SpinnerViewController* spinner;

    @end

    @implementation WelcomeViewController

    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.

        self.spinner = [SpinnerViewController alloc];
        [self.spinner startWithContainer:self];

        self.userProfilePhoto.image = [UIImage imageNamed:@"DefaultUserPhoto"];

        [GraphManager.instance
         getMeWithCompletionBlock:^(MSGraphUser * _Nullable user, NSError * _Nullable error) {
            dispatch_async(dispatch_get_main_queue(), ^{
                [self.spinner stop];

                if (error) {
                    // Show the error
                    UIAlertController* alert = [UIAlertController
                                                alertControllerWithTitle:@"Error getting user profile"
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

                // Set display name
                self.userDisplayName.text = user.displayName ? : @"Mysterious Stranger";
                [self.userDisplayName sizeToFit];

                // AAD users have email in the mail attribute
                // Personal accounts have email in the userPrincipalName attribute
                self.userEmail.text = user.mail ? : user.userPrincipalName;
                [self.userEmail sizeToFit];
            });
         }];
    }

    - (IBAction)signOut {
        [AuthenticationManager.instance signOut];
        [self performSegueWithIdentifier: @"userSignedOut" sender: nil];
    }

    @end
    ```

1. <span data-ttu-id="5634c-144">Execute o aplicativo, entre e toque na guia **calendário** . Você deve ver a lista de eventos.</span><span class="sxs-lookup"><span data-stu-id="5634c-144">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/calendar-list.png)
