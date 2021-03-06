<properties
	pageTitle="Introdução ao Linux no Azure | Microsoft Azure"
	description="Saiba como usar máquinas virtuais Linux no Azure."
	services="virtual-machines-linux"
	documentationCenter="python"
	authors="szarkos"
	manager="timlt"
	editor=""
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/01/2016"
	ms.author="szark"/>

#Introdução ao Linux no Azure

Este tópico apresenta uma visão geral de alguns aspectos do uso de máquinas virtuais Linux na nuvem do Azure. Implantar uma máquina virtual Linux é um processo simples usando uma imagem da galeria.


## Autenticação: nomes de usuário, senhas e chaves SSH.

Ao criar uma máquina virtual Linux usando o portal clássico do Azure, você deve fornecer um nome de usuário, senha ou uma chave pública SSH. A escolha de um nome de usuário para a implantação de uma máquina virtual Linux no Azure está sujeita à seguinte restrição: nomes de contas (UID <100) do sistema já presentes na máquina virtual não são permitidos - root por exemplo.


 - Confira [Criar uma máquina virtual que executa Linux](virtual-machines-linux-cli-create.md)
 - Consulte [Como usar SSH com Linux no Azure](virtual-machines-linux-ssh-from-linux.md)


## Obtendo privilégios de superusuário usando o `sudo`

A conta de usuário especificada durante a implantação da instância de máquina virtual no Azure é uma conta privilegiada. Essa conta é configurada pelo Agente Linux do Azure para poder elevar privilégios para raiz (conta de superusuário) usando o utilitário `sudo`. Depois de fazer logon usando essa conta de usuário, você poderá executar comandos como raiz usando a sintaxe de comando.

	# sudo <COMMAND>

Opcionalmente, você pode obter um shell de root usando **sudo -s**.

- Consulte [Usando privilégios de raiz em máquinas virtuais Linux do Azure](virtual-machines-linux-use-root-privileges.md)


## Configuração do firewall

O Azure fornece um filtro de pacote de entrada que restringe a conectividade a portas especificadas no portal clássico do Azure. Por padrão, a única porta permitida é SSH. Você pode abrir o acesso a portas adicionais na sua máquina virtual Linux configurando pontos de extremidade no portal clássico do Azure:

 - Confira: [Como instalar pontos de extremidade em uma máquina virtual](virtual-machines-windows-classic-setup-endpoints.md)

As imagens do Linux na Galeria do Azure não habilitam o firewall *iptables* por padrão. Se desejado, o firewall poderá ser configurado para fornecer filtragem adicional.


## Alterações de nome do host

Ao implantar uma instância de uma imagem do Linux inicialmente, você precisa fornecer um nome de host para a máquina virtual. Quando a máquina virtual está em execução, esse nome de host é publicado nos servidores DNS da plataforma, de forma que as várias máquinas virtuais conectadas entre si possam executar pesquisas de endereço IP usando nomes de host.

Se forem desejadas alterações no nome do host depois da implantação de uma máquina virtual, use o comando

	# sudo hostname <newname>

O Agente Linux do Azure inclui uma funcionalidade para detectar automaticamente essa alteração de nome e configurar corretamente a máquina virtual para persistir nessa alteração e, além disso, publicá-la nos servidores DNS da plataforma.

 - [Guia do usuário do agente Linux para o Azure](virtual-machines-linux-agent-user-guide.md)

### Inicialização de nuvem
As imagens do **Ubuntu** e **CoreOS** utilizam inicialização de nuvem pn Azure, que fornece recursos adicionais para inicializar uma máquina virtual.

 - [Como injetar dados personalizados](virtual-machines-windows-classic-inject-custom-data.md)
 - [Dados personalizados e inicialização de nuvem no Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Criar partições de troca do Azure usando a nuvem Init](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Como usar o CoreOS no Azure](virtual-machines-linux-classic-coreos-howto.md)


## Captura de imagem da máquina virtual

O Azure oferece a possibilidade de capturar o estado de uma máquina virtual existente em uma imagem que pode ser usada depois na implantação de instâncias de máquina virtual. O Agente Linux do Azure pode ser usado para reverter algumas das personalizações que foram realizadas durante o processo de provisionamento. Você pode seguir as seguintes etapas para capturar uma máquina virtual como uma imagem:

1. Execute **waagent-deprovision** para desfazer a personalização do provisionamento. Ou **waagent -deprovision+user** para, opcionalmente, excluir a conta de usuário especificada durante o provisionamento e todos os dados associados.

2. Desligue a máquina virtual.

3. Clique em *Capturar* no portal clássico do Azure ou use as ferramentas Powershell ou CLI para capturar a máquina virtual como uma imagem.

 - Confira: [Como capturar uma máquina virtual Linux para ser usada como um modelo](virtual-machines-linux-classic-capture-image.md)


## Anexando discos

Cada máquina virtual tem um *disco de recursos* anexado. Como os dados em um disco de recurso talvez não sejam duráveis nas reinicializações, ele costuma ser usado por aplicativos e processos em execução na máquina virtual para o armazenamento de dados transitório e **temporário**. Ele também é usado para armazenar páginas ou trocar arquivos para o sistema operacional.

No Linux, o disco de recurso é normalmente gerenciado pelo agente do Linux do Azure e montado automaticamente em **/mnt/resource** (ou **/mnt** nas imagens do Ubuntu).


>[AZURE.NOTE] Observe que o disco de recurso é um disco **temporário** e pode ser excluído e reformatado quando a VM é reinicializada.

No Linux, o disco de dados pode ser nomeado pelo kernel como `/dev/sdc`, e os usuários precisarão particionar, formatar e montar esse recurso. Isso é abordado passo a passo no tutorial: [Como anexar um disco de dados a uma máquina virtual](virtual-machines-linux-classic-attach-disk.md).

 - **Consulte também:** [configurar RAID de software no Linux](virtual-machines-linux-configure-raid.md)

<!---HONumber=AcomDC_0323_2016-->