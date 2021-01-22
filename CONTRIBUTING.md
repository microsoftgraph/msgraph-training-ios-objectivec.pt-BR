# <a name="contributing-to-microsoft-graph-training-repositories"></a><span data-ttu-id="6ecfe-101">Contribuição para repositórios de treinamento do Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="6ecfe-101">Contributing to Microsoft Graph training repositories</span></span>

<span data-ttu-id="6ecfe-102">Obrigado por contribuir com este projeto!</span><span class="sxs-lookup"><span data-stu-id="6ecfe-102">Thank you for contributing to this project!</span></span> <span data-ttu-id="6ecfe-103">Antes de enviar sua solicitação pull, considere o seguinte.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-103">Before submitting your pull request, be sure to consider the following.</span></span>

## <a name="overview"></a><span data-ttu-id="6ecfe-104">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="6ecfe-104">Overview</span></span>

<span data-ttu-id="6ecfe-105">O código neste repositório tem três finalidades:</span><span class="sxs-lookup"><span data-stu-id="6ecfe-105">The code in this repository serves three purposes:</span></span>

- <span data-ttu-id="6ecfe-106">Os arquivos markdown na pasta [do tutorial](/tutorial) são publicados como um tutorial na página de [tutoriais do Microsoft Graph.](https://docs.microsoft.com/graph/tutorials)</span><span class="sxs-lookup"><span data-stu-id="6ecfe-106">The Markdown files in the [tutorial](/tutorial) folder are published as a tutorial on the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) page.</span></span>
- <span data-ttu-id="6ecfe-107">O projeto de exemplo na pasta [de demonstração](/demo) é a origem de um [início rápido do Microsoft Graph.\*](https://developer.microsoft.com/graph/quick-start) *\** _</span><span class="sxs-lookup"><span data-stu-id="6ecfe-107">The sample project in the [demo](/demo) folder is the source for a [Microsoft Graph quick start](https://developer.microsoft.com/graph/quick-start).\**\** _</span></span>
- <span data-ttu-id="6ecfe-108">O projeto de exemplo na pasta de demonstração também pode ser baixado diretamente do GitHub e deve ser executado como está após alguma configuração simples.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-108">The sample project in the demo folder is also downloadable directly from GitHub and should run as-is after some simple configuration.</span></span>

> <span data-ttu-id="6ecfe-109">_*\**_ Nem todos os repositórios de treinamento estão disponíveis como inícios rápidos (ainda).</span><span class="sxs-lookup"><span data-stu-id="6ecfe-109">_*\**_ Not all training repositories are available as quick starts (yet).</span></span>

<span data-ttu-id="6ecfe-110">É importante ter isso em mente, porque as alterações em um _may\* exigem alterações em outro, para manter tudo em sincronia. Sempre que possível, os arquivos Markdown se referem aos arquivos de código-fonte diretamente (usando uma sintaxe personalizada), para que a atualização do código na fonte atualize automaticamente o código `:::code` no Markdown.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-110">This is important to keep in mind, because changes in one place _may\* require changes in another, to keep things in sync. Whereever possible, the Markdown files refer to the source code files directly (using a custom `:::code` syntax), so that updating code in source will automatically update the code in Markdown.</span></span>

## <a name="updating-code"></a><span data-ttu-id="6ecfe-111">Atualizando código</span><span class="sxs-lookup"><span data-stu-id="6ecfe-111">Updating code</span></span>

<span data-ttu-id="6ecfe-112">A `:::code` sintaxe usada no Markdown depende de comentários específicos no arquivo de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-112">The `:::code` syntax used in Markdown depends on specific comments in the source code file.</span></span> <span data-ttu-id="6ecfe-113">Esses comentários são parecidos com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6ecfe-113">These comments look like the following:</span></span>

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

<span data-ttu-id="6ecfe-114">Se você atualizar o código entre esses comentários de "marcador", os arquivos markdown receberão automaticamente essas alterações quando publicados no site de documentação do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-114">If you update code between these "marker" comments, the Markdown files will automatically get those changes when published to the Microsoft Graph documentation site.</span></span> <span data-ttu-id="6ecfe-115">Se você atualizar o código fora desses comentários, é muito provável que você precisará atualizar o Markdown correspondente.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-115">If you update code outside of those comments, it's very likely that you'll need to update the corresponding Markdown.</span></span>

## <a name="adding-features"></a><span data-ttu-id="6ecfe-116">Adicionando recursos</span><span class="sxs-lookup"><span data-stu-id="6ecfe-116">Adding features</span></span>

<span data-ttu-id="6ecfe-117">Embora o entusiasmo seja agradecedo, não envie solicitações pull para adicionar novos recursos ao exemplo.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-117">While the enthusiasm is appreciated, please don't send pull requests to add new features to the sample.</span></span> <span data-ttu-id="6ecfe-118">Como esse repositório é principalmente um tutorial de "criar seu primeiro aplicativo", o conjunto de recursos é limitado, por design.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-118">Because this repository is primarily a "build your first app" tutorial, the feature set is limited, by design.</span></span>

## <a name="submitting-pull-requests"></a><span data-ttu-id="6ecfe-119">Enviando solicitações pull</span><span class="sxs-lookup"><span data-stu-id="6ecfe-119">Submitting pull requests</span></span>

<span data-ttu-id="6ecfe-120">Envie todas as solicitações pull para a `master` filial.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-120">Please submit all pull requests to the `master` branch.</span></span>

## <a name="when-do-changes-get-published"></a><span data-ttu-id="6ecfe-121">Quando as alterações são publicadas?</span><span class="sxs-lookup"><span data-stu-id="6ecfe-121">When do changes get published?</span></span>

<span data-ttu-id="6ecfe-122">A publicação de atualizações no site [de tutoriais do Microsoft Graph](https://docs.microsoft.com/graph/tutorials) não é automática.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-122">Publishing of updates to the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) site is not automatic.</span></span> <span data-ttu-id="6ecfe-123">As alterações devem ser promovidas primeiro para a filial e, em seguida, um build deve `live` ser acionado pelos administradores do site.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-123">Changes must first be promoted to the `live` branch, then a build must be triggered by the site admins.</span></span> <span data-ttu-id="6ecfe-124">Isso geralmente é feito "conforme necessário".</span><span class="sxs-lookup"><span data-stu-id="6ecfe-124">This is typically done on an "as-needed" basis.</span></span>

## <a name="code-of-conduct"></a><span data-ttu-id="6ecfe-125">Código de conduta</span><span class="sxs-lookup"><span data-stu-id="6ecfe-125">Code of conduct</span></span>

<span data-ttu-id="6ecfe-p106">Este projeto adotou o [Código de Conduta do Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/). Para saber mais, confira as [Perguntas frequentes do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou contate [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.</span><span class="sxs-lookup"><span data-stu-id="6ecfe-p106">This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.</span></span>
