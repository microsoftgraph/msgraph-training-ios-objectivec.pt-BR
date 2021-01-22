<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph ao aplicativo. Para este aplicativo, você usará o [SDK](https://github.com/microsoftgraph/msgraph-sdk-objc) do Microsoft Graph para o Objetivo C para fazer chamadas ao Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

Nesta seção, você estenderá a classe para adicionar uma função para obter os eventos do usuário para a semana atual e atualizar para `GraphManager` `CalendarViewController` usar essa nova função.

1. Abra **GraphManager.h** e adicione o seguinte código acima da `@interface` declaração.

    ```objc
    typedef void (^GetCalendarViewCompletionBlock)(NSData* _Nullable data,
                                                   NSError* _Nullable error);
    ```

1. Adicione o código a seguir à `@interface` declaração.

    ```objc
    - (void) getCalendarViewStartingAt: (NSString*) viewStart
                              endingAt: (NSString*) viewEnd
                   withCompletionBlock: (GetCalendarViewCompletionBlock) completion;
    ```

1. Abra **GraphManager.m** e adicione a função a seguir à `GraphManager` classe.

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
    > Considere o que o código `getCalendarViewStartingAt` está fazendo.
    >
    > - A URL que será chamada é `/v1.0/me/calendarview` .
    >   - Os `startDateTime` `endDateTime` parâmetros e consulta definem o início e o fim da exibição de calendário.
    >   - O `select` parâmetro de consulta limita os campos retornados para cada evento a apenas aqueles que o ponto de exibição realmente usará.
    >   - O `orderby` parâmetro de consulta classifica os resultados por hora de início.
    >   - O `top` parâmetro de consulta solicita 25 resultados por página.
    >   - o header faz com que o Microsoft Graph retorne as horas de início e término de cada evento `Prefer: outlook.timezone` no fuso horário do usuário.

1. Crie uma nova **classe Cocoa Touch** no projeto **GraphTutorial** chamado **GraphToIana**. Escolha **NSObject** na **Subclasse do** campo.
1. Abra **GraphToIana.h** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.h" id="GraphToIanaSnippet":::

1. Abra **GraphToIana.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.m" id="GraphToIanaSnippet":::

    Isso faz uma pesquisa simples para encontrar um identificador de fuso horário IANA com base no nome do fuso horário retornado pelo Microsoft Graph.

1. Abra **CalendarViewController.m e** substitua seu conteúdo inteiro pelo código a seguir.

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

1. Execute o aplicativo, entre e toque no item **de** navegação calendário no menu. Você deverá ver um despejo JSON dos eventos no aplicativo.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode substituir o despejo JSON por algo para exibir os resultados de maneira amigável. Nesta seção, você modificará a função para retornar objetos fortemente digitados e modificará para usar um exibição de tabela para `getCalendarViewStartingAt` `CalendarViewController` renderizar os eventos.

### <a name="update-getcalendarviewstartingat"></a>Atualizar getCalendarViewStartingAt

1. Abra **GraphManager.h.** Altere `GetCalendarViewCompletionBlock` a definição de tipo para o seguinte.

    ```objc
    typedef void (^GetCalendarViewCompletionBlock)(NSArray<MSGraphEvent*>* _Nullable events, NSError* _Nullable error);
    ```

1. Abra **GraphManager.m.** Substitua a função `getCalendarViewStartingAt` existente pelo código seguinte.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/GraphManager.m" id="GetCalendarViewSnippet" highlight="42-61":::

### <a name="update-calendarviewcontroller"></a>Atualizar CalendarViewController

1. Crie um novo **arquivo de classe Cocoa Touch** no projeto **GraphTutorial** chamado `CalendarTableViewCell` . Escolha **UITableViewCell** na **Subclasse do** campo.
1. Abra **CalendarTableViewCell.h** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.h" id="CalendarTableCellSnippet":::

1. Abra **CalendarTableViewCell.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.m" id="CalendarTableCellSnippet":::

1. Crie um novo **arquivo de classe Cocoa Touch** no projeto **GraphTutorial** chamado `CalendarTableViewController` . Escolha **UITableViewController** na **Subclasse do** campo.
1. Abra **CalendarTableViewController.h e** substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.h" id="CalendarTableViewControllerSnippet":::

1. Abra **CalendarTableViewController.m** e substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.m" id="CalendarTableViewControllerSnippet":::

1. Abra **Main.storyboard** e localize a **cena do calendário.** Exclua o visualizador de rolagem do visualização raiz.
1. Usando a **Biblioteca,** adicione uma **Barra de Navegação** à parte superior do modo de exibição.
1. Clique duas vezes no **título** na barra de navegação e atualize-o para `Calendar` .
1. Usando a **biblioteca**, adicione um **item de** botão de barra ao lado direito da barra de navegação.
1. Selecione o novo botão de barra e selecione o **Inspetor de Atributos.** Alterar **Imagem para** **mais**.
1. Adicione um **modo de exibição** de contêiner **da Biblioteca** ao modo de exibição na barra de navegação. Reize a exibição do contêiner para que pegue todo o espaço restante no ponto de vista.
1. Definir restrições na barra de navegação e no modo de exibição de contêiner da seguinte forma.

    - **Barra de Navegação**
        - Adicionar restrição: Altura, valor: 44
        - Restrição Adicionar: Espaço à frente da Área de Segurança, valor: 0
        - Adicionar restrição: Espaço à trailing para a Área de Segurança, valor: 0
        - Restrição Adicionar: Espaço superior à Área de Segurança, valor: 0
    - **Exibição de Contêiner**
        - Restrição Adicionar: Espaço à frente da Área de Segurança, valor: 0
        - Adicionar restrição: Espaço à trailing para a Área de Segurança, valor: 0
        - Restrição Adicionar: Espaço superior à Barra de Navegação Inferior, valor: 0
        - Adicionar restrição: Espaço inferior à Área de Segurança, valor: 0

1. Localize o segundo controlador de exibição adicionado ao storyboard quando você adicionou o ponto de vista do contêiner. Ele é conectado à **cena do calendário** por um segue de insusuidade. Selecione este controlador e use o **Inspetor de Identidade** para alterar a **classe** para **CalendarTableViewController**.
1. Exclua **o view** do controlador **de exibição de tabela de calendário.**
1. Adicione um **exibição de tabela** da **biblioteca ao** controlador **de exibição de tabela de calendário.**
1. Selecione o exibição de tabela e selecione o **Inspetor de Atributos.** Definir **células de protótipo** como **1**.
1. Arraste a borda inferior da célula de protótipo para lhe dar uma área maior para trabalhar.
1. Use a **Biblioteca para** adicionar três **Rótulos** à célula de protótipo.
1. Selecione a célula de protótipo e, em seguida, selecione o **Inspetor de Identidade.** Alterar **classe** para **CalendarTableViewCell**.
1. Selecione o **Inspetor de Atributos** e de definir o **identificador** como `EventCell` .
1. Com o **EventCell** selecionado, selecione o Inspetor de Conexões e conecte-se, e aos rótulos que você adicionou à célula no  `durationLabel` `organizerLabel` `subjectLabel` storyboard.
1. De definidas as propriedades e restrições nos três rótulos da seguinte forma.

    - **Rótulo de Assunto**
        - Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0
        - Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0
        - Adicionar restrição: Espaço superior à Margem Superior do Exibição de Conteúdo, valor: 0
    - **Rótulo do Organizador**
        - Fonte: Sistema 12.0
        - Restrição Adicionar: Altura, valor: 15
        - Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0
        - Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0
        - Adicionar restrição: Espaço superior para o Rótulo de Assunto Inferior, valor: Padrão
    - **Rótulo de Duração**
        - Fonte: Sistema 12.0
        - Cor: Cor Cinza Escuro
        - Restrição Adicionar: Altura, valor: 15
        - Adicionar restrição: espaço à esquerda do visualização de conteúdo à margem esquerda, valor: 0
        - Adicionar restrição: espaço à direita para a margem à direita do visualização de conteúdo, valor: 0
        - Restrição Adicionar: Espaço superior para o Rótulo do Organizador Inferior, valor: Padrão
        - Adicionar restrição: Espaço inferior à Margem Inferior do Exibição de Conteúdo, valor: 0

1. Select the **EventCell**, then select the **Size Inspector**. **Habilitar Automático** para **Altura da Linha.**

    ![Uma captura de tela dos controladores de exibição de calendário e tabela de calendário](images/calendar-table-storyboard.png)

1. Abra **CalendarViewController.h** e remova a `calendarJSON` propriedade.

1. Abra **CalendarViewController.m e** substitua seu conteúdo pelo código a seguir.

    :::code language="objc" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.m" id="CalendarViewControllerSnippet":::

1. Execute o aplicativo, entre e toque na **guia** Calendário. Você deverá ver a lista de eventos.

    ![Uma captura de tela da tabela de eventos](./images/calendar-list.png)
