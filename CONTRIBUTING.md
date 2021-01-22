# <a name="contributing-to-microsoft-graph-training-repositories"></a>Contribuição para repositórios de treinamento do Microsoft Graph

Obrigado por contribuir com este projeto! Antes de enviar sua solicitação pull, considere o seguinte.

## <a name="overview"></a>Visão Geral

O código neste repositório tem três finalidades:

- Os arquivos markdown na pasta [do tutorial](/tutorial) são publicados como um tutorial na página de [tutoriais do Microsoft Graph.](https://docs.microsoft.com/graph/tutorials)
- O projeto de exemplo na pasta [de demonstração](/demo) é a origem de um [início rápido do Microsoft Graph.*](https://developer.microsoft.com/graph/quick-start) *\** _
- O projeto de exemplo na pasta de demonstração também pode ser baixado diretamente do GitHub e deve ser executado como está após alguma configuração simples.

> _*\**_ Nem todos os repositórios de treinamento estão disponíveis como inícios rápidos (ainda).

É importante ter isso em mente, porque as alterações em um _may* exigem alterações em outro, para manter tudo em sincronia. Sempre que possível, os arquivos Markdown se referem aos arquivos de código-fonte diretamente (usando uma sintaxe personalizada), para que a atualização do código na fonte atualize automaticamente o código `:::code` no Markdown.

## <a name="updating-code"></a>Atualizando código

A `:::code` sintaxe usada no Markdown depende de comentários específicos no arquivo de código-fonte. Esses comentários são parecidos com o seguinte:

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

Se você atualizar o código entre esses comentários de "marcador", os arquivos markdown receberão automaticamente essas alterações quando publicados no site de documentação do Microsoft Graph. Se você atualizar o código fora desses comentários, é muito provável que você precisará atualizar o Markdown correspondente.

## <a name="adding-features"></a>Adicionando recursos

Embora o entusiasmo seja agradecedo, não envie solicitações pull para adicionar novos recursos ao exemplo. Como esse repositório é principalmente um tutorial de "criar seu primeiro aplicativo", o conjunto de recursos é limitado, por design.

## <a name="submitting-pull-requests"></a>Enviando solicitações pull

Envie todas as solicitações pull para a `master` filial.

## <a name="when-do-changes-get-published"></a>Quando as alterações são publicadas?

A publicação de atualizações no site [de tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) não é automática. As alterações devem ser promovidas primeiro para a filial e, em seguida, um build deve `live` ser acionado pelos administradores do site. Isso geralmente é feito "conforme necessário".

## <a name="code-of-conduct"></a>Código de conduta

Este projeto adotou o [Código de Conduta do Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/). Para saber mais, confira as [Perguntas frequentes do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou contate [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.
