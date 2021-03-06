<properties 
	pageTitle="Solução de problemas de ausência de dados - Application Insights para .NET" 
	description="Não consegue ver os dados no Application Insights do Visual Studio? Tente aqui." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/29/2016" 
	ms.author="awills"/>
 
# Solução de problemas de ausência de dados - Application Insights para .NET


## Problemas do Monitor de Status

*Eu [instalei o Monitor de Status](app-insights-monitor-performance-live-website-now.md) no meu servidor Web para monitorar os aplicativos existentes. Não consigo ver todos os resultados.*

Veja [Solução de problemas do Monitor de Status](app-insights-monitor-performance-live-website-now.md#troubleshooting). Portas de firewall são o problema mais comum.


## <a name="q01"></a>A opção “Adicionar o Application Insights” não existe no Visual Studio

*Ao criar um novo projeto no Visual Studio ou quando clico duas vezes no projeto existente no Gerenciador de Soluções, não vejo opções do Application Insights.*

+ Nem todos os tipos de projeto do .NET têm suporte das ferramentas. Há suporte para projetos Web e WCF. Para outros tipos de projeto, como aplicativos de área de trabalho ou de serviço, você ainda pode [adicionar manualmente um SDK do Application Insights ao seu projeto](app-insights-windows-desktop.md).
+ Certifique-se de que você tem o [Visual Studio 2013 Atualização 3 ou posterior](http://go.microsoft.com/fwlink/?LinkId=397827). Ele vem pré-instalado com as Ferramentas do Application Insights.
+ Escolha **Ferramentas**, **Extensões e Atualizações** e verifique se as **Ferramentas do Application Insights** estão instaladas e habilitadas. Nesse caso, clique em **Atualizações** para ver se há uma atualização disponível.
+ Abra o diálogo Novo Projeto e escolha aplicativo Web ASP.NET. Se você vir a opção de Application Insights, as ferramentas estão instaladas. Caso contrário, tente desinstalar e reinstale as Ferramentas do Application Insights.


## <a name="q02"></a>Falha ao adicionar o Application Insights

*Ao criar um novo projeto Web ou ao tentar adicionar o Application Insights a um projeto existente, vejo uma mensagem de erro.*

Causas mais prováveis:

* A comunicação com o portal do Application Insights tiver falhado ou
* Há algum problema com sua conta do Azure;
* Você tiver apenas [acesso de leitura para a assinatura ou grupo em que está tentando criar o novo recurso](app-insights-resources-roles-access-control.md).

Correção:

+ Verifique se você forneceu as credenciais corretas de entrada da conta do Azure. 
+ No navegador, verifique se você tem acesso ao [portal do Azure](https://portal.azure.com). Abra Configurações e veja se há alguma restrição.
+ [Adicione o Application Insights ao seu projeto existente](app-insights-asp-net.md): no Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione “Adicionar Application Insights”.
+ Se ainda não estiver funcionando, siga o [procedimento manual](app-insights-start-monitoring-app-health-usage.md) para adicionar um recurso no portal e, em seguida, adicione o SDK ao seu projeto. 

## <a name="emptykey"></a>Recebo um erro "Chave de instrumentação não pode ser vazio"

Parece que algo deu errado enquanto você instalava o Application Insights, ou talvez um adaptador de registro em log.

No Gerenciador de Soluções, clique com o botão direito do mouse em `ApplicationInsights.config` e escolha **Configurar o Application Insights**. Você obterá uma caixa de diálogo que solicita que você entre no Azure e crie um recurso de Application Insights, ou então reutilize um recurso existente.


##<a name="NuGetBuild"></a> "Pacotes NuGet estão ausentes" no meu servidor de compilação

*Tudo é compilado corretamente quando eu estou depurando em meu computador de desenvolvimento, mas obtenho um erro do NuGet no servidor de compilação.*

Consulte [Restauração do Pacote NuGet](http://docs.nuget.org/Consume/Package-Restore) e [Restauração Automática do Pacote](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## Comando de menu ausente para abrir o Application Insights do Visual Studio

*Quando eu clico com o botão direito do mouse no Gerenciador de Soluções do meu projeto, não vejo os comandos do Application Insights ou não vejo um comando Abrir Application Insights.*

Causas mais prováveis:

* Se você tiver criado o recurso Application Insights manualmente, ou se o projeto for de um tipo que não seja compatível com as ferramentas do Application Insights.
* As ferramentas do Application Insights estão desabilitadas no Visual Studio.
* O Visual Studio é anterior à versão 2013 Atualização 3.

Correção:

* Verifique se a versão do Visual Studio é a 2013 atualização 3 ou posterior.
* Escolha **Ferramentas**, **Extensões e Atualizações** e verifique se as **Ferramentas do Application Insights** estão instaladas e habilitadas. Nesse caso, clique em **Atualizações** para ver se há uma atualização disponível.
* Clique com o botão direito do mouse no projeto no Gerenciador de Soluções. Se você vir o comando **Configurar Application Insights**, use-o para conectar seu projeto ao recurso no serviço Application Insights.


Caso contrário, o tipo de projeto não terá suporte direto das ferramentas do Application Insights. Para ver a telemetria, entre no [portal do Azure](https://portal.azure.com), escolha Application Insights na barra de navegação à esquerda e selecione seu aplicativo.

## “Acesso negado” ao abrir o Application Insights a partir do Visual Studio

*O comando de menu “Abrir Application Insights” me leva ao portal do Azure, mas eu recebo um erro de “acesso negado”.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

A entrada da Microsoft que você usou pela última vez no navegador padrão não tem acesso ao [recurso que foi criado quando o Application Insights foi adicionado a este aplicativo](app-insights-asp-net.md). Há duas razões prováveis:

* Você tem mais de uma conta da Microsoft. Talvez uma conta de trabalho e uma conta pessoal da Microsoft? A entrada usada pela última vez no navegador padrão foi para uma conta diferente daquela que tem acesso a [adicionar o Application Insights ao projeto](app-insights-asp-net.md). 

 * Correção: clique em seu nome no canto superior direito da janela do navegador e saia. Em seguida, entre com a conta que tem acesso. Na barra de navegação à esquerda, clique em Application Insights e escolha seu aplicativo.

* Alguém adicionou o Application Insights ao projeto e esqueceu-se de dar [acesso ao grupo de recursos](app-insights-resources-roles-access-control.md) no qual ele foi criado.

 * Correção: se eles tiverem usado uma conta organizacional, poderão adicionar você à equipe, ou podem conceder acesso individual ao grupo de recursos.



## “Ativo não encontrado” ao abrir o Application Insights do Visual Studio

*O comando de menu “Abrir Application Insights” me leva ao portal do Azure, mas eu obtenho um erro de “ativo não encontrado”.*

Causas mais prováveis:

* O recurso Application Insights para seu aplicativo foi excluído ou
* A chave de instrumentação foi definida ou alterada em applicationinsights.config quando ele foi editado diretamente sem a atualização do arquivo do projeto. 

A chave de instrumentação em applicationinsights.config controla o local para onde a telemetria é enviada. Uma linha no arquivo do projeto controla qual recurso será aberto quando você usar o comando no Visual Studio.

Correção:

* No Gerenciador de Soluções, clique com o botão direito do mouse no projeto no Gerenciador de Soluções e escolha Application Insights, Configurar Application Insights. No diálogo, você pode optar por enviar a telemetria para um recurso existente ou criar um novo. Ou:
* Abra o recurso diretamente. Entre no [portal do Azure](https://portal.azure.com), clique em Application Insights na barra de navegação à esquerda e selecione seu aplicativo.



## Onde posso encontrar minha telemetria?

*Entrei no [portal do Microsoft Azure](https://portal.azure.com) e estou olhando para o painel inicial do Azure. Como eu encontro meus dados no Application Insights?*

* Na barra de navegação à esquerda, clique em Application Insights e no nome do aplicativo. Se você não tiver projetos, precisará [adicionar ou configurar o Application Insights ao seu projeto Web](app-insights-asp-net.md).

    Lá, você verá alguns gráficos de resumo. Clique em qualquer gráfico para ver mais detalhes.

* No Visual Studio, enquanto estiver depurando o aplicativo, clique no botão Application Insights.


## <a name="q03"></a> Nenhum dado do servidor (ou nenhum dado)

*Executei meu aplicativo e, em seguida, abri o serviço Application Insights no Microsoft Azure, mas todos os gráficos mostram “Saiba como coletar...” ou “Não configurado”.* Ou *somente Exibição de Página e dados de usuário, mas nenhum dado do servidor.*

+ Execute seu aplicativo em modo de depuração no Visual Studio (F5). Use o aplicativo para gerar alguma telemetria. Verifique se você pode ver os eventos registrados na janela de saída do Visual Studio. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ No portal do Application Insights, abra a [Pesquisa de Diagnóstico](app-insights-diagnostic-search.md). Os dados normalmente aparecem aqui primeiro.
+ Clique no botão Atualizar. A folha se atualiza periodicamente, mas você também pode fazê-lo manualmente. O intervalo de atualização é maior para intervalos de tempo maiores.
+ Verifique se as chaves de instrumentação correspondem. Na folha principal do aplicativo no portal do Application Insights, na lista suspensa **Essentials**, examine **Chave de instrumentação**. Em seguida, no projeto no Visual Studio, abra applicationinsights.config e localize a `<instrumentationkey>`. Verifique se as duas chaves são iguais. Caso contrário:
 + No portal, clique em Application Insights e procure o recurso de aplicativo com a chave correta ou
 + No Gerenciador de Soluções do Visual Studio, clique com o botão direito do mouse no projeto e escolha Application Insights, Configurar. Redefina o aplicativo para enviar telemetria ao recurso correto.
 + Se você não conseguir localizar as chaves correspondentes, verifique se está usando as mesmas credenciais de logon no Visual Studio e no portal.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ No [painel inicial do Microsoft Azure](https://portal.azure.com), veja o mapa de Integridade do Serviço. Se houver indicações de alerta, espere até que elas tenham voltado a OK; então, feche e abra novamente a folha do Application Insights de seu aplicativo.
+ Verifique também o [nosso blog de status](http://blogs.msdn.com/b/applicationinsights-status/).
+ Você escreveu algum código para o [SDK do lado do servidor](app-insights-api-custom-events-metrics.md) que possa alterar a chave de instrumentação em instâncias do `TelemetryClient` ou no `TelemetryContext`? Ou escreveu uma [configuração de filtro ou de amostragem](app-insights-api-filtering-sampling.md) que possa estar filtrando em excesso?
+ Se você tiver editado o ApplicationInsights.config, verifique cuidadosamente a configuração de [TelemetryInitializers e TelemetryProcessors](app-insights-api-filtering-sampling.md). Um tipo ou parâmetro nomeado incorretamente pode fazer com que o SDK não envie nenhum dado.

## <a name="q04"></a>Nenhum dado sobre Exibições de Página, Navegadores, Uso

*Vejo dados nos gráficos Tempo de Resposta do Servidor e Solicitações de Servidor, mas nenhum dado em tempo de Carregamento da Exibição de Página ou nas folhas Navegador ou Uso.*

Os dados vêm de scripts nas páginas da Web.

+ Se tiver adicionado o Application Insights a um projeto Web existente, [você terá que adicionar os scripts manualmente](app-insights-javascript.md).
+ Tenha certeza de que o Internet Explorer não está exibindo o site no modo de Compatibilidade.
+ Use o recurso de depuração do navegador (F12 em alguns navegadores e, em seguida, escolha Rede) para verificar se os dados estão sendo enviados para `dc.services.visualstudio.com`.

## Nenhum dado de dependência ou de exceção

Confira [telemetria de dependência](app-insights-asp-net-dependencies.md) e [telemetria de exceção](app-insights-asp-net-exceptions.md).

## Nenhum dado (servidor) desde que publiquei o aplicativo no servidor

+ Verifique que você copiou todos da Microsoft. DLLs do ApplicationInsights no servidor, junto com Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ Em seu firewall, você terá que abrir as portas TCP 80 e 443 para tráfego de saída de dc.services.visualstudio.com e f5.services.visualstudio.com.
+ Se você tiver que usar um proxy para envio fora de sua rede corporativa, defina [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) em Web.config
+ Windows Server 2008: certifique-se de ter instalado as seguintes atualizações: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## Eu costumava ver os dados, mas eles foram interrompidos

* Verifique o [blog de status](http://blogs.msdn.com/b/applicationinsights-status/).
* Você atingiu sua cota mensal de pontos de dados? Abra configurações/Cota e Preços para descobrir. Nesse caso, você pode atualizar seu plano ou então pagar por capacidade adicional. Consulte o [esquema de preços](https://azure.microsoft.com/pricing/details/application-insights/).


## Não vejo todos os dados que eu esperava

Se o aplicativo enviar muitos dados e se você estiver usando o SDK do Application Insights para o ASP.NET versão 2.0.0-beta3 ou posterior, o recurso de [amostragem adaptável](app-insights-sampling.md) poderá operar e enviar apenas uma porcentagem de sua telemetria.

Você pode desativá-lo, mas isso não é recomendado. A amostragem é projetada para que a telemetria relacionada seja corretamente transmitida para fins de diagnóstico.

## Dados geográficos errados na telemetria do usuário

A dimensões de cidade, de região e de país são derivadas de endereços IP e nem sempre são precisas.

## Exceção "método não encontrado" em execução nos Serviços de Nuvem do Azure

Você compilou para .NET 4.6? O 4.6 não tem suporte automático nas funções dos Serviços de Nuvem do Azure. [Instale o 4.6 em cada função](../cloud-services/cloud-services-dotnet-install-dotnet.md) antes de executar seu aplicativo.

## Ainda não está funcionando...

* [Fórum do Application Insights](https://social.msdn.microsoft.com/Forums/vstudio/pt-BR/home?forum=ApplicationInsights)

<!---HONumber=AcomDC_0224_2016-->
