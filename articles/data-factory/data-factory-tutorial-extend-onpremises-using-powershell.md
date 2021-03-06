<properties 
	pageTitle="Passo a passo: copiar dados de saída para um banco de dados do SQL Server (Azure PowerShell)" 
	description="Este passo a passo estende o tutorial sobre como usar o Azure PowerShell, de modo que o pipeline copie dados de saída para um banco de dados do SQL Server."
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/01/2016" 
	ms.author="spelluru"/>


# Passo a passo: copiar dados de saída para um banco de dados local do SQL Server (Azure PowerShell)
Neste passo a passo, você aprenderá a configurar o ambiente para habilitar o pipeline para trabalhar com dados locais.
 
Na última etapa do cenário de processamento de log do primeiro passo a passo com Partição -> Enriquecer -> Analisar fluxo de trabalho, a saída de eficácia de campanha marketing foi copiada para um banco de dados SQL do Azure. Você também pode mover esses dados para o SQL Server local para análise dentro da sua organização.
 
Para copiar dados de eficácia de campanha de marketing do Blob do Azure para o SQL Server local, você precisa criar Serviço Vinculado local, Tabela e Pipeline adicionais usando o mesmo conjunto de cmdlets introduzidos no primeiro passo a passo.

Você **deve** executar o passo a passo no [Tutorial: Mover e processar arquivos de log usando Data Factory][datafactorytutorial] antes de executar o passo a passo deste artigo.

**(recomendado)** Examine e pratique o passo a passo no artigo [Habilitar seus pipelines para trabalhar com dados locais][useonpremisesdatasources] para ver um passo a passo sobre como criar um pipeline para mover dados do SQL Server local para um armazenamento de blob do Azure.


Neste tutorial, você realizará as seguintes etapas:

1. [Criar um gateway de gerenciamento de dados](#create-data-management-gateway). O Gateway de Gerenciamento de Dados é um agente cliente que fornece acesso a fontes de dados locais de sua organização na nuvem. O gateway permite que a transferência de dados entre um SQL Server local e armazenamentos de dados do Azure.	

	Você deve ter pelo menos um gateway instalado no seu ambiente corporativo, bem como registrá-lo com o Data Factory do Azure antes de adicionar o banco de dados SQL Server local como um serviço vinculado a um data factory do Azure.

2. [Criar um serviço vinculado do SQL Server](#create-sql-server-linked-service). Nesta etapa, você primeiro cria um banco de dados e uma tabela no computador local do SQL Server e, em seguida, cria o serviço vinculado: **OnPremSqlLinkedService**.
3. [Criar um conjunto de dados e um pipeline](#create-dataset-and-pipeline). Nesta etapa, você criará uma tabela **MarketingCampaignEffectivenessOnPremSQLTable** e um pipeline **EgressDataToOnPremPipeline**. 

4. [Monitorar o pipeline](#monitor-pipeline). Nesta etapa, você monitorará as fatias de dados, tabelas e pipelines usando o Portal Clássico do Azure.


## Criar um Gateway de Gerenciamento de Dados

O Gateway de Gerenciamento de Dados é um agente cliente que fornece acesso a fontes de dados locais de sua organização na nuvem. O gateway permite que a transferência de dados entre um SQL Server local e armazenamentos de dados do Azure.
  
Você deve ter pelo menos um gateway instalado no seu ambiente corporativo, bem como registrá-lo com o Data Factory do Azure antes de adicionar o banco de dados SQL Server local como um serviço vinculado a um data factory do Azure.

Se você tiver um gateway de dados existente que você possa usar, ignore esta etapa.

1.	Crie um gateway de dados lógicos. No **Portal do Azure**, clique em **Serviços Vinculados** na folha **DATA FACTORY**.
2.	Clique em **Adicionar (+) Gateway de Dados** na barra de comandos.  
3.	Na folha **Novo gateway de dados**, clique em **CRIAR**.
4.	Na folha **Criar**, insira **MyGateway** para o **nome** do gateway de dados.
5.	Clique em **ESCOLHER UMA REGIÃO** e altere-a se necessário. 
6.	Clique em **OK** na folha **Criar**. 
7.	Você deve ver a folha **Configurar**. 
8.	Na folha **Configurar**, clique em **Instalar diretamente neste computador**. Isso vai baixar, instalar e configurar o gateway em seu computador e registrar o gateway com o serviço. Se você tiver um gateway existente instalado no computador que você deseja vincular a esse gateway lógico no portal, use a chave nessa folha para registrar novamente o gateway usando a ferramenta Gerenciador de Configuração de Gateway de gerenciamento de dados (visualização).

	![Gerenciador de Configuração de Gateway de Gerenciamento de Dados][image-data-factory-datamanagementgateway-configuration-manager]

9. Clique em **OK** para fechar a folha **Configurar** e em **OK** para fechar a folha **Criar**. Aguarde até que o status de **MeuGateway** na folha **Serviços Vinculados** mude para **BOM**. Você também pode iniciar a ferramenta **Gerenciador de Configuração de Gateway de gerenciamento de dados (visualização)** para confirmar se o nome do gateway corresponde ao nome no portal e o **status** é **Registrado**. Você terá que fechar e reabrir a folha Serviços Vinculados para ver o status mais recente. Pode levar alguns minutos antes que a tela seja atualizada com o status mais recente.

## Criar um serviço vinculado do SQL Server

Nesta etapa, você primeiro cria o banco de dados e a tabela necessários no computador local do SQL Server e, em seguida, cria o serviço vinculado.

### Preparar tabela e banco de dados local

Para começar, você precisa criar o banco de dados SQL Server, a tabela, os tipos definidos pelo usuário e procedimentos armazenados. Esses serão usados para mover os resultados **MarketingCampaignEffectiveness** do blob do Azure para o banco de dados SQL Server.

1.	No **Windows Explorer**, navegue até a subpasta **OnPremises** na pasta **C:\\ADFWalkthrough** (ou o local no qual você extraiu os exemplos).
2.	Abra **prepareOnPremDatabase&Table.ps1** em seu editor favorito, substitua a parte destacada pelas informações do SQL Server e salve o arquivo (forneça os detalhes de **autenticação do SQL**). Para o tutorial, habilite a autenticação do SQL para o banco de dados. 
			
		$dbServerName = "<servername>"
		$dbUserName = "<username>"
		$dbPassword = "<password>"

3. No **Azure PowerShell**, navegue até a pasta **C:\\ADFWalkthrough\\OnPremises**.
4.	Execute **prepareOnPremDatabase&Table.ps1** **(entre aspas duplas ou conforme mostrado abaixo)**.
			
		& '.\prepareOnPremDatabase&Table.ps1'

5. Depois que o script for executado com êxito, você verá o seguinte:
			
		PS E:\ADF\ADFWalkthrough\OnPremises> & '.\prepareOnPremDatabase&Table.ps1'
		6/10/2014 10:12:33 PM Script to create sample on-premises SQL Server Database and Table
		6/10/2014 10:12:33 PM Creating the database [MarketingCampaigns], table and stored procedure on [.]...
		6/10/2014 10:12:33 PM Connecting as user [sa]
		6/10/2014 10:12:33 PM Summary:
		6/10/2014 10:12:33 PM 1. Database 'MarketingCampaigns' created.
		6/10/2014 10:12:33 PM 2. 'MarketingCampaignEffectiveness' table and stored procedure 


### Criar o serviço vinculado

1.	No **Portal do Azure**, clique no bloco **Serviços Vinculados** na folha **DATA FACTORY** de **LogProcessingFactory**.
2.	Na folha **Serviços Vinculados**, clique em **Adicionar (+) armazenamento de dados**.
3.	Na folha **Novo armazenamento de dados**, insira **OnPremSqlLinkedService** como o **Nome**. 
4.	Clique em **Tipo (Configurações necessárias)** e selecione **SQL Server**. Agora você deve ver as configurações de **GATEWAY DE DADOS**, **Servidor**, **Banco de dados** e **CREDENCIAIS** na folha **Novo armazenamento de dados**. 
5.	Clique em **GATEWAY DE DADOS (definir configurações necessárias)** e selecione o **MyGateway** criado anteriormente. 
6.	Insira o **nome** do servidor de banco de dados que hospeda o banco de dados **MarketingCampaigns**. 
7.	Insira **MarketingCampaigns** para o Banco de dados. 
8.	Clique em **CREDENCIAIS**. 
9.	Na folha **Credenciais**, clique em **Clique aqui para definir credenciais com segurança**.
10.	Ela instala um aplicativo em um único clique pela primeira vez e inicia a caixa de diálogo **Definindo Credenciais**.
11.	Na caixa de diálogo **Definindo Credenciais**, insira **Nome de Usuário** e **Senha**, e clique em **OK**. Aguarde até que a caixa de diálogo seja fechada. 
12.	Clique em **OK** na folha **Novo armazenamento de dados**. 
13.	Na folha **Serviços Vinculados**, confirme se **OnPremSqlLinkedService** aparece na lista e o **status** do serviço vinculado é **Bom**.

## Criar um conjunto de dados e um pipeline

### Criar a tabela lógica local

1.	No **PowerShell do Azure**, alterne para a pasta **C:\\ADFWalkthrough\\OnPremises**. 
2.	Use o cmdlet **New-AzureRmDataFactoryDataset** para criar as Tabelas para **MarketingCampaignEffectivenessOnPremSQLTable.json**, como descrito a seguir.

			
		New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\MarketingCampaignEffectivenessOnPremSQLTable.json
	 
#### Criar o pipeline para copiar os dados do Blob do Azure para o SQL Server

1.	Use o cmdlet **New-AzureRmDataFactoryPipeline** para criar o Pipeline para **EgressDataToOnPremPipeline.json**, conforme descrito a seguir.

			
		New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\EgressDataToOnPremPipeline.json
	 
2. Use o cmdlet **Set-AzureRmDataFactoryPipelineActivePeriod** para especificar o período ativo para **EgressDataToOnPremPipeline**.

			
		Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z -EndDateTime 2014-05-05Z –Name EgressDataToOnPremPipeline

	Pressione **‘Y’** para continuar.
	
## Monitorar o pipeline

Agora é possível usar as mesmas etapas introduzidas em [Monitorar pipelines](data-factory-tutorial-using-powershell.md#monitor-pipelines) para monitorar o novo pipeline e as fatias de dados da nova tabela ADF local.
 
Quando você ver o status de uma fatia da tabela **MarketingCampaignEffectivenessOnPremSQLTable** mudar para Pronto, isso significa que o pipeline concluiu a execução da fatia. Para exibir os resultados, consulte a tabela **MarketingCampaignEffectiveness** no banco de dados **MarketingCampaigns** no seu SQL Server.
 
Parabéns! Você verificou com êxito o passo a passo para usar sua fonte de dados local.
 

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[datafactorytutorial]: data-factory-tutorial-using-powershell.md
[adfgetstarted]: data-factory-get-started.md
[adfintroduction]: data-factory-introduction.md
[useonpremisesdatasources]: data-factory-move-data-between-onprem-and-cloud.md

[azure-portal]: http://portal.azure.com
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[sqlcmd-install]: http://www.microsoft.com/download/details.aspx?id=35580
[azure-sql-firewall]: http://msdn.microsoft.com/library/azure/jj553530.aspx

[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[adfwalkthrough-download]: http://go.microsoft.com/fwlink/?LinkId=517495
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx


[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-extend-onpremises-using-powershell/DataManagementGatewayConfigurationManager.png

 

<!---HONumber=AcomDC_0323_2016-->