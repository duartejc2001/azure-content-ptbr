<properties
   pageTitle="Criar e modificar um circuito da Rota Expressa usando o Gerenciador de Recursos e o PowerShell | Microsoft Azure"
   description="Este artigo descreve como criar e provisionar um circuito da Rota Expressa. Ele também mostra como verificar o circuito, como atualizá-lo ou como excluí-lo e desprovisioná-lo."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/03/2016"
   ms.author="cherylmc"/>

# Criar e modificar um circuito da Rota Expressa usando o Gerenciador de Recursos e o PowerShell

   > [AZURE.SELECTOR]
   [PowerShell - Classic](expressroute-howto-circuit-classic.md)
   [PowerShell - Resource Manager](expressroute-howto-circuit-arm.md)

Este artigo descreve como criar um circuito da Rota Expressa do azure usando os cmdlets do Windows PowerShell e o modelo de implantação do Azure Resource Manager. As etapas a seguir também mostrarão a você como verificar o status do circuito, como atualizá-lo ou como excluí-lo e desprovisioná-lo.

   [AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)]

## Pré-requisitos de configuração

Para criar um circuito da Rota Expressa, você precisará:

- Obter a versão mais recente dos módulos do Azure PowerShell, versão 1.0 ou posterior. Para obter as diretrizes passo a passo sobre como configurar seu computador para usar os módulos do PowerShell, siga as instruções da página [Como instalar e configurar o Azure PowerShell](../powershell-install-configure.md).
- Examine a página [Pré-requisitos](expressroute-prerequisites.md) e a página [Fluxos de trabalho](expressroute-workflows.md) antes de iniciar a configuração.

## Criar e provisionar um circuito da Rota Expressa

**Etapa 1. Importe o módulo do PowerShell para a Rota Expressa.**

Para começar a usar os cmdlets da Rota Expressa, você deverá instalar o instalador mais recente do PowerShell da [galeria do PowerShell](http://www.powershellgallery.com/) e importe os módulos do Gerenciador de Recursos para a sessão do PowerShell. Você precisará executar o PowerShell como administrador.

```
Install-Module AzureRM

Install-AzureRM
```

Importe todos os módulos do AzureRM que estejam no intervalo de versão semântica conhecida:

```
Import-AzureRM
```

Você também pode importar um módulo selecionado dentro do intervalo de versão semântica conhecida:

```
Import-Module AzureRM.Network
```

Entre na sua conta:

```
Login-AzureRmAccount
```

Selecione a assinatura na qual você deseja criar um circuito da Rota Expressa:

```
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"   			
```

**Etapa 2. Obtenha a lista de provedores, de locais e de larguras de banda com suporte.**

Antes de criar um circuito da Rota Expressa você precisará de uma lista de provedores de conectividade, dos locais com suporte e de opções de largura de banda. O cmdlet *Get-AzureRmExpressRouteServiceProvider* do PowerShell retorna essas informações, que serão usadas em etapas posteriores.

```
PS C:\> Get-AzureRmExpressRouteServiceProvider
```

Verifique se o provedor de conectividade está listado. Anote as informações a seguir, você precisará delas posteriormente ao criar um circuito:

- Nome
- PeeringLocations
- BandwidthsOffered

Agora você está pronto para criar um circuito da Rota Expressa.

**Etapa 3. Crie um circuito da Rota Expressa.**

Se você ainda não tiver um grupo de recursos, deverá criar um antes de criar o circuito da Rota Expressa. Faça isso ao executar o seguinte comando:

```
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

O exemplo a seguir mostra como criar um circuito da Rota Expressa de 200 Mbps por meio da Equinix, no Vale do Silício. Se você estiver usando um provedor diferente e configurações diferentes, substitua essas informações ao fazer a sua solicitação. A seguir, um exemplo de solicitação de uma nova chave de serviço:

```
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Especifique a camada da SKU e a família de SKUs corretas:

- A camada da SKU determina se um complemento padrão ou premium da Rota Expressa está habilitado. Você pode especificar o *padrão* para obter a SKU padrão ou o *premium* para o complemento premium
- A família da SKU determina o tipo de cobrança. Você pode selecionar *metereddata* para um plano de dados limitado e *unlimiteddata* para um plano de dados ilimitado. **Observação:** não será possível alterar o tipo de cobrança depois de criar um circuito.

A resposta conterá a chave de serviço. Você pode obter descrições detalhadas de todos os parâmetros ao executar o seguinte:

```
get-help New-AzureRmExpressRouteCircuit -detailed
```

**Etapa 4. Lista todos os circuitos da Rota Expressa.**

Para obter uma lista de todos os circuitos da Rota Expressa criados, execute o comando *Get-AzureRmExpressRouteCircuit*:

```
#Getting service key
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

A resposta será semelhante ao seguinte exemplo:

```
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
   		                           }
CircuitProvisioningState          : Enabled
ServiceProviderProvisioningState  : NotProvisioned
ServiceProviderNotes              :
ServiceProviderProperties         : {
                                      "ServiceProviderName": "Equinix",
                                      "PeeringLocation": "Silicon Valley",
                                      "BandwidthInMbps": 200
                                    }
ServiceKey                        : **************************************
Peerings                          : []
```

Você pode recuperar essas informações a qualquer momento usando o cmdlet *Get-AzureRmExpressRouteCircuit*. Fazer a chamada sem parâmetros listará todos os circuitos. Sua chave de serviço estará listada no campo *ServiceKey*.

```
Get-AzureRmExpressRouteCircuit
```

A resposta será semelhante ao seguinte exemplo:

```
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
   		                           }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Equinix",
                                     "PeeringLocation": "Silicon Valley",
                                     "BandwidthInMbps": 200
   		                           }
ServiceKey                       : **************************************
Peerings                         : []
```

Você pode obter descrições detalhadas de todos os parâmetros ao executar o seguinte:

```
get-help Get-AzureRmExpressRouteCircuit -detailed
```

**Etapa 5. Envie a chave de serviço ao seu provedor de conectividade para obter o provisionamento.**

Quando você criar um novo circuito da Rota Expressa, ele estará no seguinte estado:

```
ServiceProviderProvisioningState : NotProvisioned

CircuitProvisioningState         : Enabled
```

O *ServiceProviderProvisioningState* fornece informações sobre o estado atual do provisionamento no lado do provedor de serviço e o Status fornece o estado no lado da Microsoft. Para que você consiga usar um circuito da Rota Expressa, ele deverá estar no seguinte estado:

```
ServiceProviderProvisioningState : Provisioned

CircuitProvisioningState         : Enabled
```

O circuito assumirá o estado a seguir quando o provedor de conectividade estiver no processo de habilitá-lo para você:

```
ServiceProviderProvisioningState : Provisioned

Status                           : Enabled
```

**Etapa 6: Verifique periodicamente o status e o estado da chave do circuito.**

A verificação do status e o estado da chave do circuito informará você quando seu provedor tiver habilitado seu circuito. Após a configuração do circuito, o *ServiceProviderProvisioningState* será exibido como *Provisionado*, como mostrado neste exemplo:

```
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

A resposta será semelhante ao seguinte exemplo:

```
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Equinix",
                                     "PeeringLocation": "Silicon Valley",
                                     "BandwidthInMbps": 200
   	                            }
ServiceKey                       : **************************************
Peerings                         : []

```

**Etapa 7.  Crie sua configuração de roteamento.**

Para obter instruções passo a passo, consulte a [configuração do roteamento de circuito da Rota Expressa (criar e modificar os emparelhamentos de circuito)](expressroute-howto-routing-arm.md).

**Etapa 8.  Vincule uma rede virtual a um circuito da Rota Expressa.**

NEm seguida, vincule uma rede virtual a seu circuito da Rota Expressa. Você pode usar [esse modelo](https://github.com/Azure/azure-quickstart-templates/tree/ecad62c231848ace2fbdc36cbe3dc04a96edd58c/301-expressroute-circuit-vnet-connection) ao trabalhar com o modo de implantação do Gerenciador de Recursos. Atualmente, estamos trabalhando nas etapas para fazer isso usando o PowerShell.

## Obter o status de um circuito da Rota Expressa

Você pode recuperar essas informações a qualquer momento usando o cmdlet *Get-AzureRmExpressRouteCircuit*. Fazer a chamada sem parâmetros listará todos os circuitos.

```
Get-AzureRmExpressRouteCircuit
```

A resposta será semelhante ao seguinte exemplo:

```
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
   		                             "ServiceProviderName": "Equinix",
   		                             "PeeringLocation": "Silicon Valley",
   		                             "BandwidthInMbps": 200
   		                           }
ServiceKey                       : **************************************
Peerings                         : []
```

Você pode obter informações sobre um circuito da Rota Expressa específico passando o nome do grupo de recursos e o nome do circuito como um parâmetro para a chamada:

```
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

A resposta será semelhante ao seguinte exemplo:

```
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
   		                             "Tier": "Standard",
   		                             "Family": "MeteredData"
   		                           }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Equinix",
                                     "PeeringLocation": "Silicon Valley",
                                     "BandwidthInMbps": 200
   		                           }
ServiceKey                       : **************************************
Peerings                         : []
```

Você pode obter descrições detalhadas de todos os parâmetros ao executar o seguinte:

```
get-help get-azurededicatedcircuit -detailed
```

## Modificar um circuito da Rota Expressa

Você pode modificar certas propriedades de um circuito da Rota Expressa sem afetar a conectividade.

Você pode fazer o seguinte, sem tempo de inatividade:

- Como habilitar ou desabilitar o complemento premium da Rota Expressa para seu circuito da Rota Expressa.
- Aumente a largura de banda de seu circuito da Rota Expressa.

Para obter mais informações sobre limites e limitações, consulte a página [Perguntas frequentes sobre a Rota Expressa](expressroute-faqs.md).

### Habilitar o complemento premium da Rota Expressa

Você pode habilitar o complemento premium da Rota Expressa para o circuito existente usando o seguinte trecho do PowerShell:

```
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Agora, o circuito terá os recursos de complemento premium da Rota Expressa habilitados. Observe que a Microsoft começará a cobrar pelo recurso de complemento premium assim que o comando for executado com êxito.

### Desabilitar o complemento premium da Rota Expressa

Você pode desabilitar o complemento premium da Rota Expressa para o circuito existente usando o seguinte cmdlet do PowerShell:

```
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Agora, o complemento premium está desabilitado para o circuito.

Observe que esta operação poderá falhar se você estiver usando recursos que ultrapassam o que é permitido para o circuito padrão.

- Antes de você fazer o downgrade de premium para standard, verifique se o número de redes virtuais vinculadas ao circuito é menor que 10. Se você não fizer isso, sua solicitação de atualização falhará e a Microsoft cobrará com tarifas premium.
- Você precisa desvincular todas as redes virtuais em outras regiões geopolíticas. Se você não fizer isso, sua solicitação de atualização falhará e a Microsoft cobrará com tarifas premium.
- Sua tabela de roteamento deve ter menos de 4.000 rotas para o emparelhamento privado. Se o tamanho da tabela de roteamento for maior que 4.000 rotas, a sessão BGP será descartada e não poderá ser reabilitada até que o número de prefixos anunciados fique abaixo de 4.000.

### Atualizar a largura de banda do circuito da Rota Expressa

Para obter opções de largura de banda com suporte para seu provedor, verifique a página [Perguntas frequentes sobre a Rota Expressa](expressroute-faqs.md). Você pode escolher qualquer tamanho maior do que o tamanho do circuito existente. Depois de decidir sobre qual tamanho você precisa, use o seguinte comando para redimensionar o circuito:

```
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

O circuito será dimensionado no lado da Microsoft. Entre em contato com seu provedor de conectividade para que ele atualize as configurações para corresponder a essa alteração.Depois de fazer essa notificação, a Microsoft começará a cobrar pela opção de largura de banda atualizada.

**Importante**: Não é possível reduzir a largura de banda de um circuito da Rota Expressa sem interrupções. O downgrade da largura de banda exige o desprovisionamento do circuito da Rota Expressa e um reprovisionamento de um novo circuito da Rota Expressa.

## Excluir e desprovisionar um circuito da Rota Expressa

Você pode excluir o circuito da Rota Expressa executando o comando a seguir:

```
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

Observe que você precisa desvincular todas as redes virtuais do circuito da Rota Expressa para que essa operação tenha sucesso. Se essa operação falhar, verifique se há redes virtuais vinculadas ao circuito.

Se o estado de provisionamento do provedor de serviços do circuito da Rota Expressa estiver habilitado, o status passará de habilitado para *desabilitando*. Trabalhe com seu provedor de serviços para desprovisionar o circuito no lado dele. A Microsoft continuará a reservar recursos e a cobrar de você até que o provedor de serviços complete o desprovisionamento do circuito e nos notifique.

Se o provedor de serviços tiver desprovisionado o circuito (o estado de provisionamento do provedor de serviços estiver definido como *não provisionado*) antes da execução do cmdlet acima, a Microsoft desprovisionará o circuito e interromperemos a cobrança.

## Próximas etapas

Depois de criar seu circuito, faça o seguinte:

- [Criar e modificar o roteamento do circuito da Rota Expressa](expressroute-howto-routing-arm.md)
- [Vincular a rede virtual ao circuito da Rota Expressa](expressroute-howto-linkvnet-arm.md)


<!----HONumber=AcomDC_0309_2016-->

