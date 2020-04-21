<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará o [SDK do Microsoft Graph para o objetivo C](https://github.com/microsoftgraph/msgraph-sdk-objc) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

Nesta seção, você estenderá a `GraphManager` classe para adicionar uma função para obter os eventos e a atualização `CalendarViewController` do usuário para usar essas novas funções.

1. Abra **graphmanager. h** e adicione o seguinte código acima da `@interface` declaração.

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSData* _Nullable data, NSError* _Nullable error);
    ```

1. Adicione o seguinte código à `@interface` declaração.

    ```objc
    - (void) getEventsWithCompletionBlock: (GetEventsCompletionBlock)completionBlock;
    ```

1. Abra **graphmanager. m** e adicione a função a seguir à `GraphManager` classe.

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
    > Considere o que o código `getEventsWithCompletionBlock` está fazendo.
    >
    > - A URL que será chamada é `/v1.0/me/events`.
    > - O `select` parâmetro de consulta limita os campos retornados para cada evento para apenas aqueles que o aplicativo usará realmente.
    > - O `orderby` parâmetro de consulta classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

1. Abra **CalendarViewController. m** e substitua todo o conteúdo pelo código a seguir.

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

1. Execute o aplicativo, entre e toque no item de navegação de **calendário** no menu. Você deve ver um despejo JSON dos eventos no aplicativo.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode substituir o despejo JSON por algo para exibir os resultados de forma amigável. Nesta seção, você irá modificar a `getEventsWithCompletionBlock` função para retornar objetos fortemente tipados e modificar `CalendarViewController` para usar um modo de exibição de tabela para renderizar os eventos.

### <a name="update-getevents"></a>Atualizar GetEvents

1. Abra o **graphmanager. h**. Altere a `GetEventsCompletionBlock` definição de tipo para o seguinte.

    ```objc
    typedef void (^GetEventsCompletionBlock)(NSArray<MSGraphEvent*>* _Nullable events, NSError* _Nullable error);
    ```

1. Abra o **graphmanager. m**. Substitua a `completionBlock(data, nil);` linha na `getEventsWithCompletionBlock` função com o código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphManager.m" id="GetEventsSnippet" highlight="24-43":::

### <a name="update-calendarviewcontroller"></a>Atualizar CalendarViewController

1. Crie um novo arquivo de **classe Touch** do Cocoa **GraphTutorial** no projeto GraphTutorial `CalendarTableViewCell`chamado. Escolha **UITableViewCell** na **subclasse de** Field.
1. Abra **CalendarTableViewCell. h** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.h" id="CalendarTableCellSnippet":::

1. Abra **CalendarTableViewCell. m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.m" id="CalendarTableCellSnippet":::

1. Abra **Main. Storyboard** e localize a **cena do calendário**. Selecione o **modo de exibição** na **cena do calendário** e exclua-o.

    ![Uma captura de tela do modo de exibição da cena de calendário](./images/view-in-calendar-scene.png)

1. Adicione um **modo de exibição de tabela** da **biblioteca** à **cena do calendário**.
1. Selecione o modo de exibição de tabela e, em seguida, selecione o **Inspetor de atributos**. Definir **células de protótipo** como **1**.
1. Use a **biblioteca** para adicionar três **Rótulos** à célula prototype.
1. Selecione a célula Prototype e, em seguida, selecione o **Inspetor de identidade**. Altere a **classe** para **CalendarTableViewCell**.
1. Selecione o **Inspetor de atributos** e defina o `EventCell` **identificador** como.
1. Com o **EventCell** selecionado, selecione o **Inspetor de conexões** e `durationLabel`Conecte `organizerLabel`-se `subjectLabel` , e para os rótulos que você adicionou à célula no storyboard.
1. Defina as propriedades e as restrições nos três rótulos da seguinte maneira.

    - **Rótulo de assunto**
        - Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0
        - Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0
        - Adicionar restrição: espaço superior à margem superior do modo de exibição de conteúdo, valor: 0
    - **Rótulo do organizador**
        - Fonte: System 12,0
        - Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0
        - Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0
        - Adicionar restrição: espaço superior para rótulo de assunto inferior, valor: padrão
    - **Rótulo de duração**
        - Fonte: System 12,0
        - Cor: cor cinza escuro
        - Adicionar restrição: espaço à esquerda na margem esquerda da exibição de conteúdo, valor: 0
        - Adicionar restrição: espaço à direita à margem à direita da exibição do conteúdo, valor: 0
        - Adicionar restrição: espaço superior ao rótulo do organizador inferior, valor: padrão
        - Adicionar restrição: espaço inferior à margem inferior da visualização de conteúdo, valor: 8

    ![Uma captura de tela do layout da célula de protótipo](./images/prototype-cell-layout.png)

1. Abra **CalendarViewController. h** e remova a `calendarJSON` propriedade.
1. Altere a `@interface` declaração para o seguinte.

    ```objc
    @interface CalendarViewController : UITableViewController
    ```

1. Abra **CalendarViewController. m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.m" id="CalendarViewSnippet":::

1. Execute o aplicativo, entre e toque na guia **calendário** . Você deve ver a lista de eventos.

    ![Uma captura de tela da tabela de eventos](./images/calendar-list.png)
