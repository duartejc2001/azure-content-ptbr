<properties
   pageTitle="Criar uma rede virtual com uma conexão de Gateway de VPN site a site usando o Portal Clássico do Azure | Microsoft Azure"
   description="Criar uma Rede Virtual com uma conexão de Gateway de VPN site a site para configurações entre locais e híbridas usando o modelo de implantação clássico."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/16/2016"
   ms.author="cherylmc"/>

# Criar uma rede virtual com uma conexão VPN site a site usando o portal clássico do Azure

> [AZURE.SELECTOR]
- [Portal do Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Portal do Azure - Clássico](vpn-gateway-site-to-site-create.md)
- [PowerShell – Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


Este artigo o orientará pela criação de uma rede virtual e de uma conexão VPN site a site com sua rede local. As conexões site a site podem ser usadas para configurações entre instalações e híbridas. Este artigo se aplica ao modelo de implantação clássico e usa o portal clássico do Azure.

**Sobre modelos de implantação do Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]
 
![Diagrama Site a Site](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

**Modelos de implantação e ferramentas para conexões Site a Site**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]
 
Se quiser conectar VNets, mas não estiver criando uma conexão com uma instalação local, confira [Configurar uma conexão de VNet a VNet para o modelo de implantação clássico](virtual-networks-configure-vnet-to-vnet-connection.md) ou [Configurar uma conexão de VNet para VNet para o modelo de implantação do Gerenciador de Recursos](vpn-gateway-vnet-vnet-rm-ps.md).

 
## Antes de começar

Verifique se você tem os itens a seguir antes de iniciar a configuração.

- Um dispositivo VPN compatível e alguém que possa configurá-lo. Confira [Sobre dispositivos VPN](vpn-gateway-about-vpn-devices.md). Se não estiver familiarizado com a configuração do dispositivo VPN ou com os intervalos de endereços IP localizados na configuração de rede local, você precisará coordenar-se com alguém que possa lhe fornecer os detalhes.

-  Um endereço IP público voltado para o exterior para seu dispositivo VPN. Esse endereço IP não pode estar localizado atrás de um NAT.

- Uma assinatura do Azure. Se ainda não tiver uma assinatura do Azure, você poderá ativar os [Benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou se inscrever para obter uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).


## Criar sua rede virtual

1. Faça logon no [portal clássico do Azure](https://manage.windowsazure.com/).

2. No canto inferior esquerdo da tela, clique em **Novo**. No painel de navegação, clique em **Serviços de Rede** e, em seguida, clique em **Rede Virtual**. Clique em **Criação Personalizada** para iniciar o assistente de configuração.

3. Preencha as informações nas páginas a seguir para criar sua rede virtual.

## Página Detalhes da Rede Virtual

Insira as seguintes informações.

- **Nome**: nome da sua rede virtual. Por exemplo, *EastUSVNet*. Você usará esse nome de rede virtual quando implantar suas VMs e instâncias de PaaS. Portanto, é melhor não criar um nome muito complicado.
- **Local**: o local está diretamente relacionado ao local físico (região) onde você deseja que os recursos (VMs) residam. Por exemplo, se você desejar que as VMs implantadas nesta rede virtual estejam localizadas fisicamente no *leste dos EUA*, selecione esse local. Você não pode alterar a região associada à sua rede virtual depois de criá-la.

## Página Servidores DNS e Conectividade VPN

Insira as informações a seguir e, em seguida, clique na seta de avanço no canto inferior direito.

- **Servidores DNS**: insira o nome do servidor DNS e o endereço IP ou selecione um servidor DNS previamente registrado no menu de atalho. Essa configuração não cria um servidor DNS. Ela permite que você especifique os servidores DNS que deseja usar para a resolução de nomes para essa rede virtual.
- **Configurar VPN Site a Site**: marque a caixa de seleção **Configurar uma VPN site a site**.
- **Rede Local**: uma rede local representa sua localização física no local. Você pode selecionar uma rede local já criada anteriormente ou pode criar uma nova rede local. No entanto, se você optar por usar uma rede local criada anteriormente, vai querer acessar a página de configuração **Redes Locais** e garantir que o endereço IP do Dispositivo VPN (endereço IPv4 voltado para o público) para o dispositivo VPN usado nesta conexão seja preciso.

## Página Conectividade Site a Site

Se estiver criando uma nova rede local, você verá a página **Conectividade Site a Site**. Se você quiser usar uma rede local criada anteriormente, essa página não será exibida no assistente e você poderá ir para a próxima seção.

Insira as informações a seguir e, em seguida, clique na seta de avanço.

- 	**Nome**: o nome que você deseja dar ao site de rede local.
- 	**Endereço IP do Dispositivo VPN**: é o endereço IPv4 voltado para o público do seu dispositivo VPN local que você usará para conectar-se ao Azure. O dispositivo VPN não pode ser localizado por trás de um NAT.
- 	**Espaço de Endereço**: inclua o IP Inicial e a Contagem de Endereços (CIDR). É o local onde você especifica o(s) intervalo(s) de endereços que deseja que seja(m) enviado(s) por meio do gateway de rede virtual para o local. Se um endereço IP de destino estiver dentro dos intervalos especificados aqui, ele será roteado por meio do gateway de rede virtual.
- 	**Adicionar espaço de endereço**: se houver vários intervalos de endereços que deseje enviar pelo gateway da rede virtual, é o local onde você especificará cada intervalo de endereços adicionais. Você poderá adicionar ou remover intervalos posteriormente na página **Rede Local**.

## Página Espaços de endereço de rede virtual

Especifique o intervalo de endereços que você deseja usar para sua rede virtual. Esses são os DIPS (endereços IP dinâmicos) que serão atribuídos às VMs e a outras instâncias de função que você implantar nessa rede virtual.

É particularmente importante selecionar um intervalo que não se sobreponha a nenhum dos intervalos usados para sua rede local. Você precisará coordenar com o administrador da rede. Talvez seu administrador da rede precise reservar um intervalo de endereços IP de seu espaço de endereço de rede local para que você possa usar para sua rede virtual.

Insira as informações a seguir e clique na marca de seleção no canto inferior direito para configurar sua rede.

- **Espaço de Endereço**: inclui o IP Inicial e a Contagem de Endereços. Verifique se os espaços de endereço especificados não se sobrepõem a qualquer espaço de endereço em sua rede local.
- **Adicionar sub-rede**: incluindo o IP Inicial e a Contagem de Endereços. Sub-redes adicionais não são necessárias, mas convém criar uma sub-rede separada para VMs que terão DIPS estáticos. Ou então, você pode colocar suas VMs em uma sub-rede separada das outras instâncias de função.
- **Adicionar sub-rede de gateway**: clique para adicionar a sub-rede de gateway. A sub-rede de gateway é usada apenas para o gateway de rede virtual e é necessária para esta configuração.

Clique na marca de seleção na parte inferior da página e sua rede virtual começará a ser criada. Quando ela for concluída, você verá **Criada** listada em **Status** na página **Redes** no Portal Clássico do Azure. Depois que a rede virtual tiver sido criada, você poderá configurar seu gateway de rede virtual.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## Configurar seu Gateway de Rede Virtual

Em seguida, você configurará o gateway de rede virtual para criar uma conexão VPN site a site segura. Confira [Configurar um gateway de rede virtual no portal clássico do Azure](vpn-gateway-configure-vpn-gateway-mp.md).

## Próximas etapas

Quando sua conexão for concluída, você poderá adicionar máquinas virtuais às suas redes virtuais. Confira a documentação de [Máquinas Virtuais](https://azure.microsoft.com/documentation/services/virtual-machines/) para saber mais.

<!---HONumber=AcomDC_0406_2016-->