<properties
	pageTitle="Usando o Conector SFTP em Aplicativos Lógicos | Serviço de Aplicativo do Microsoft Azure"
	description="Como criar e configurar o Conector SFTP ou o aplicativo de API e usá-lo em um aplicativo lógico no Serviço de Aplicativo do Azure"
	authors="anuragdalmia"
	manager="erikre"
	editor=""
	services="app-service\logic"
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/16/2016"
	ms.author="sameerch"/>

# Introdução ao Conector SFTP e à adição dele ao seu Aplicativo Lógico
>[AZURE.NOTE] Esta versão do artigo aplica-se à versão do esquema 2014-12-01-preview de aplicativos lógicos. Para obter a versão do esquema 2015-08-01-preview, clique em [API do SFTP](../connectors/connectors-create-api-sftp.md).

O Conector SFTP permite mover dados de e para um servidor SFTP. Você pode baixar ou carregar arquivos ou listar arquivos de e para um servidor SFTP.

Aplicativos lógicos podem ser disparados com base em diversas fontes de dados e oferecem conectores para obter e processar dados como parte do fluxo. Você pode adicionar o Conector SFTP a seu fluxo de trabalho de negócios e processar dados como parte desse fluxo de trabalho dentro de um Aplicativo Lógico.

## Criando um conector de SFTP para seu aplicativo lógico ##
Um conector pode ser criado em um aplicativo lógico ou diretamente no Azure Marketplace. Para criar um conector no Marketplace:

1. No quadro inicial do Azure, selecione **Marketplace**.
2. Pesquise "Conector SFTP", selecione-o e selecione **Criar**.
3. Configure o Conector SFTP da seguinte maneira: ![][1]
	- **Local** - escolha a região geográfica onde você quer que o conector seja implantado
	- **Assinatura** - escolha uma assinatura na qual você deseja que esse conector seja criado
	- **Grupo de recursos** - selecione ou crie um grupo de recursos onde o conector deve residir
	- **Plano de hospedagem na Web** - selecione ou crie uma plano de hospedagem na Web
	- **Camada de preços** - escolha uma camada de preços para o conector
	- **Nome** - dê um nome ao Conector SFTP
	- **Configurações do pacote**
		- **Endereço do Servidor** - especifique o nome do servidor SFTP ou o endereço IP
		- **Aceitar Qualquer HostKey de Servidor SSH** - determina se qualquer impressão digital de chave de host público SSH do servidor deve ser aceita. Se definido como falso, a chave de host será comparada à chave especificada na propriedade "Impressão digital de chave de host de servidor SSH"
		- **HostKey de Servidor SSH** - especifique a impressão digital de chave de host público para o servidor SSH - *opcional*.
		- **Pasta Raiz** - especifique um caminho da pasta raiz. Se o espaço em branco tiver como padrão a raiz.
		- **Criptografia** - especifique a cifra de criptografia - *opcional*.
		- **Porta do Servidor** - especifique o número da porta do Servidor SFTP
4. Clique em Criar. Será criado um novo Conector SFTP.

5. Navegue até o aplicativo de API recém-criado via Procurar -> Aplicativos de API -> <Name of the API App just created> e você poderá ver que o componente "Segurança" não está configurado: ![][2]
6. Clique no componente "Segurança" para configurar a segurança (nome de usuário, senha, chave privada, senha do arquivo PPK) para o conector SFTP. Selecione a guia de autorização "Senha", "Privatekey" ou "MultiFator" em Segurança e forneça as propriedades necessárias: ![][3] ![][4] ![][5]  
6. Depois que a configuração de segurança for salva, você poderá criar um aplicativo lógico no mesmo grupo de recursos para usar o conector SFTP.

## Usando o Conector de SFTP em seu Aplicativo Lógico ##
Depois de criar seu aplicativo de API, você pode usar o conector de SFTP como gatilho/ação para seu aplicativo lógico. Para fazer isso, você precisa:

1.	Crie um novo Aplicativo Lógico e escolha o mesmo grupo de recursos que tenha o conector SFTP: ![][6]
2.	Abra "Gatilhos e Ações" para abrir o Designer de Aplicativos Lógicos e configurar seu fluxo: ![][7]
3.	O conector SFTP será exibido na seção "Aplicativos de API neste grupo de recursos" na galeria, no lado direito: ![][8]
4.	Você pode soltar o aplicativo de API do Conector SFTP no editor clicando em "Conector SFTP".

5.	Agora você pode usar o conector de SFTP no fluxo. Você pode usar o arquivo recuperado do gatilho de SFTP ("TriggerOnFileAvailable") em outras ações no fluxo.

	> [AZURE.IMPORTANT] O gatilho SFTP "TriggerOnFileAvailable" exclui o arquivo recuperado após o processamento do arquivo.

6.	Configure as propriedades de entrada de gatilho de SFTP da seguinte maneira:

	- **Caminho da Pasta** - especifique o caminho da pasta da qual os arquivos precisam ser recuperados.
	- **O tipo de arquivo: texto ou binário** - selecione o tipo do arquivo.
	- **Máscara de arquivo** - especifique a máscara de arquivo a ser aplicada para a recuperação de arquivos. ' *' recupera todos os arquivos na pasta especificada.
	- **Excluir Máscara de Arquivo** - especifique a máscara de arquivo a ser aplicada para a exclusão de arquivos. Se a propriedade "Máscara de Arquivo" também for definida, a Máscara de Exclusão de Arquivo será aplicada primeiro.


	![][9]  
	![][10]

7.	De maneira semelhante, você pode usar as ações de SFTP no fluxo. Você pode usar a ação "Carregar Arquivo" para carregar um arquivo no servidor SFTP. Configure as propriedades de entrada para a ação "Carregar Arquivo" da seguinte maneira:

	- **Conteúdo** - Especifica o conteúdo do arquivo a ser carregado
	- **Codificação de Transferência de Conteúdo** - especifique nenhuma ou Base64
	- **Caminho do Arquivo** - especifique o caminho do arquivo a ser carregado
	- **Substituir** - especifique "true" para substituir o arquivo se ele já existir
	- ****Anexar Se Existir** - especifique "true" ou "false". Quando definido como "true", os dados serão anexados ao arquivo, se ele existir. Quando definido como "false", o arquivo será substituído, se ele existir
	- **Pasta Temporária** - se fornecida, o adaptador carregará o arquivo para o “Caminho da Pasta Temporária” e, quando o carregamento for concluído, o arquivo será movido para o “Caminho da Pasta”. O “Caminho da Pasta Temporária” deve estar no mesmo disco físico que o “Caminho da Pasta” para garantir que a operação de movimentação seja atômica. A pasta temporária pode ser usada apenas quando a propriedade Anexar se Existir está desabilitada.

	![][11]
	![][12]

## Faça mais com seu Conector
Agora que o conector foi criado, você pode adicioná-lo a um fluxo de trabalho comercial usando um Aplicativo Lógico. Consulte [O que são Aplicativos Lógicos?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Se você deseja começar com os Aplicativos Lógicos do Azure antes de se inscrever em uma conta do Azure, acesse [Experimentar os Aplicativos Lógicos](https://tryappservice.azure.com/?appservice=logic), em que você pode criar imediatamente um aplicativo lógico inicial de curta duração no Serviço de Aplicativo. Não é necessário nenhum cartão de crédito; não há compromissos.

Exibir a referência da API REST de Swagger em [Conectores e referência de aplicativos de API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Você também pode examinar estatísticas de desempenho e controlar a segurança do conector. Consulte [Gerenciar e monitorar Aplicativos de API e conectores internos](app-service-logic-monitor-your-connectors.md).


<!-- Image reference -->
[1]: ./media/app-service-logic-connector-sftp/img1.PNG
[2]: ./media/app-service-logic-connector-sftp/img2.PNG
[3]: ./media/app-service-logic-connector-sftp/img3.PNG
[4]: ./media/app-service-logic-connector-sftp/img4.PNG
[5]: ./media/app-service-logic-connector-sftp/img5.PNG
[6]: ./media/app-service-logic-connector-sftp/img6.PNG
[7]: ./media/app-service-logic-connector-sftp/img7.png
[8]: ./media/app-service-logic-connector-sftp/img8.png
[9]: ./media/app-service-logic-connector-sftp/img9.PNG
[10]: ./media/app-service-logic-connector-sftp/img10.PNG
[11]: ./media/app-service-logic-connector-sftp/img11.PNG
[12]: ./media/app-service-logic-connector-sftp/img12.PNG

<!---HONumber=AcomDC_0323_2016-->