<properties
	pageTitle="Introdução aos Hubs de eventos em C e C# | Microsoft Azure"
	description="Siga este tutorial para começar a usar os Hubs de Eventos do Azure; enviar eventos em C; e receber eventos em C# usando o EventProcessorHost."
	services="event-hubs"
	documentationCenter=""
	authors="fsautomata"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="c"
	ms.devlang="csharp"
	ms.topic="article"
	ms.date="03/08/2016"
	ms.author="sethm"/>

# Introdução aos Hubs de Evento

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Introdução

Hubs de Eventos são um sistema de inclusão altamente dimensionável que pode receber milhões de eventos por segundo, permitindo que um aplicativo processe e analise grandes quantidades de dados produzidos por aplicativos e dispositivos conectados. Depois de coletados em Hubs de Evento, você pode transformar e armazenar dados usando qualquer provedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte [Visão geral de Hubs de Evento][].

Neste tutorial, você aprenderá a receber mensagens em um Hub de Eventos usando um aplicativo de console em C, e recuperá-los em paralelo usando a biblioteca do [Host do Processador de Eventos][] em C#.

Para concluir este tutorial, você precisará do seguinte:

+ Um ambiente de desenvolvimento C. Para este tutorial, vamos pressupor a pilha gcc em uma [Máquina Virtual Linux do Azure](../virtual-machines/virtual-machines-linux-cli-create.md) com Ubuntu 14.04. Serão fornecidas instruções para outros ambientes em links externos.

+ Microsoft Visual Studio Express 2013 para Windows

+ Uma conta ativa do Azure. <br/>Se você não tiver uma conta, poderá criar uma conta de avaliação gratuita em apenas alguns minutos Para obter detalhes, consulte <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fpt-BR%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Avaliação gratuita do Azure</a>.

## Criar um Hub de Evento

1. Faça logon no [portal clássico do Azure][] e clique em **NOVO** na parte inferior da tela.

2. Clique em **Serviços de Aplicativos**, em seguida, **Barramento de Serviço**, em seguida, **Hub de eventos** e, em seguida, **Criação rápida**.

	![][1]

3. Digite um nome para o Hub de Evento, selecione a região desejada e clique em **Criar um novo Hub de Evento**.

	![][2]

4. Clique no namespace que você acabou de criar (geralmente ***nome do hub de evento*-ns**).

	![][3]

5. Clique na guia **Hubs de Eventos** no início da página e clique no Hub de Evento que acabou de criar.

	![][4]

6. Clique na guia **Configurar** na parte superior da página, adicione uma regra chamada **SendRule** com direitos *Send*, adicione outra regra chamada **ReceiveRule** com direitos *Manage, Send, Listen* e clique em **Salvar**.

	![][5]

7. Na mesma página, tome nota das chaves geradas para **SendRule**.

	![][6b]

8. Clique na guia **Painel** parte superior da página e clique em **Informações de Conexão**. Anote as duas cadeias de conexão.

	![][6]

Seu Hub de Evento foi criado, e você tem as cadeias de conexão que precisa para enviar e receber eventos.

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## Executar os aplicativos

Agora você está pronto para executar os aplicativos.

1.	Execute o projeto **Receptor** no Visual Studio e aguarde que ele inicie os receptores de todas as partições.

	![][21]

2.	Execute o programa **Remetente** e veja os eventos aparecerem na janela do receptor.

	![][24]

## Próximas etapas

Agora que criou um aplicativo funcional que cria um Hub de Eventos e envia e recebe dados, você pode passar para os seguintes cenários:

- Um [aplicativo de exemplo completo que usa os Hubs de Evento][].
- O exemplo de [Escala horizontal do processamento de eventos com Hubs de Eventos][].
- Uma [solução de mensagens na fila][] usando filas do Barramento de Serviço.
- [Visão geral de Hubs de Evento][]

<!-- Images. -->
[1]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6.png
[6b]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6b.png


[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[portal clássico do Azure]: https://manage.windowsazure.com/
[Host do Processador de Eventos]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Visão geral de Hubs de Evento]: event-hubs-overview.md
[aplicativo de exemplo completo que usa os Hubs de Evento]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Escala horizontal do processamento de eventos com Hubs de Eventos]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[solução de mensagens na fila]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

<!---HONumber=AcomDC_0323_2016-->