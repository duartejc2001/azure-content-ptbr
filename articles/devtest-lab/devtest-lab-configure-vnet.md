<properties
	pageTitle="Configurar uma rede virtual para um Laboratório de Desenvolvimento/Teste | Microsoft Azure"
	description="Saiba como configurar uma rede virtual e sub-rede existente e usá-las em uma VM"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/15/2016"
	ms.author="tarcher"/>

# Configurar uma rede virtual para um Laboratório de Desenvolvimento/Teste

## Visão geral

Conforme explicado no artigo, [Adicionar uma VM com artefatos a um Laboratório de Desenvolvimento/Teste do Azure](devtest-lab-add-vm-with-artifacts.md), quando você cria uma VM em um laboratório, é possível especificar uma rede virtual (e sub-rede) configurada para essa VM. Um cenário no qual isso e possível é quando você deseja ser capaz de acessar os recursos da rede corporativa a partir de suas VMs usando a rede virtual configurada com a Rota Expressa ou VPN site a site. As seções a seguir ilustram como adicionar sua rede virtual existente às configurações de Rede Virtual do laboratório, para que ela esteja disponível para escolha durante a criação de suas VMs.

## Configurar uma rede virtual para um Laboratório de Desenvolvimento/Teste usando o Portal do Azure
As etapas a seguir orientarão você pela adição de uma rede virtual (e sub-rede) existente a um Laboratório de Desenvolvimento/Teste, para que ela possa ser usada durante a criação de uma VM no mesmo Laboratório.

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Toque em **Procurar** e toque em **Laboratórios de Desenvolvimento/Teste** na lista.

1. Na lista de laboratórios, toque no laboratório desejado.

1. A folha **Configurações** do laboratório selecionado será exibida.

1. Toque em **Redes virtuais**.

	![As redes virtuais podem ser configuradas na folha Configurações do laboratório](./media/devtest-lab-configure-vnet/lab-settings-vnet.png)
	
1. Na folha **Redes virtuais**, você verá uma lista das redes virtuais configuradas para o laboratório atual, bem como a rede virtual padrão que é criada para o laboratório.

1. Toque **+ Adicionar**.

	![Adicionar uma rede virtual existente ao seu laboratório](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
	
1. Na folha **Rede virtual**, toque em **[Selecionar rede virtual]**.

	![Selecionar uma rede virtual existente](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
	
1. Na folha **Escolher rede virtual**, selecione a rede virtual desejada. A folha mostra todas as redes virtuais que estão na mesma região da assinatura que o laboratório.

1. Depois de selecionar uma rede virtual e retornar à folha **Rede Virtual**, você verá que vários campos foram habilitados.

	![Selecionar uma rede virtual existente](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)

1. Especifique uma descrição para sua combinação de rede virtual/laboratório.

1. Para permitir o uso de uma sub-rede na criação da VM do laboratório, marque a opção **USAR NA CRIAÇÃO DA VM**.

1. Para permitir endereços IP públicos em uma sub-rede, marque a opção **PERMITIR IP PÚBLICO**.

1. No campo **MÁX. DE VMS POR USUÁRIO**, especifique o número máximo de VMs por usuário para cada sub-rede. Se você quiser um número irrestrito de VMs, deixe esse campo em branco.

1. Toque em **Salvar**.

1. Agora que a rede virtual está configurada, ela poderá ser selecionada durante a criação de uma nova VM. Isso é explicado no artigo, [Adicionar uma VM com artefatos a um Laboratório de Desenvolvimento/Teste do Azure](devtest-lab-add-vm-with-artifacts.md).

## Próximas etapas

Depois de adicionar as redes virtuais desejadas ao seu laboratório, a próxima etapa será [Adicionar uma VM ao Laboratório de Desenvolvimento/Teste](devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0323_2016-->