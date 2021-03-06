<properties
	pageTitle="Visão geral do Enterprise State Roaming | Microsoft Azure"
	description="Fornece informações sobre as configurações do Enterprise State Roaming em dispositivos do Windows. O Enterprise State Roaming fornece aos usuários uma experiência unificada em seus dispositivos Windows e reduz o tempo necessário para configurar um novo dispositivo."
	services="active-directory"
    keywords="o que é o Enterprise State Roaming, sincronização de empresa, nuvem do windows"
	documentationCenter=""
	authors="femila"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"  
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/04/2016"
	ms.author="femila"/>

# Visão geral do Enterprise State Roaming

Com o Windows 10, os usuários do [Active Directory do Azure (AD do Azure)](active-directory-whatis.md) obtêm a capacidade de sincronizar com segurança suas configurações de usuário e seus dados de configurações do aplicativo com a nuvem. O Enterprise State Roaming fornece aos usuários uma experiência unificada em seus dispositivos Windows e reduz o tempo necessário para configurar um novo dispositivo. O Enterprise State Roaming funciona de maneira semelhante à [sincronização de configurações de consumidor](http://windows.microsoft.com/pt-BR/windows-8/sync-settings-pcs) padrão, introduzida no Windows 8. Além disso, o Enterprise State Roaming oferece:

- **Separação dos dados corporativos e do consumidor** – as organizações estão no controle de seus dados, e não há nenhuma combinação de dados corporativos em uma conta de nuvem do consumidor ou de dados do consumidor em uma conta de nuvem da empresa. 
- **Segurança avançada** – os dados são criptografados automaticamente antes de deixar o dispositivo Windows 10 do usuário usando o Azure Rights Management (Azure RMS) e os dados permanecem criptografados em repouso na nuvem. Todo o conteúdo permanece criptografado em repouso na nuvem, exceto os namespaces, como os nomes de configurações e os nomes de aplicativo do Windows.  
- **Serviços de monitoramento e de gerenciamento** – mais controle e visibilidade sobre quem sincroniza configurações em sua organização e em quais dispositivos. 
- **Localização geográfica dos dados na nuvem** – os dados serão armazenados em uma região do Azure com base no país do domínio do AD do Azure. 



| Artigo | Descrição |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Habilitar o Enterprise State Roaming no Active Directory do Azure](active-directory-windows-enterprise-state-roaming-enable.md) | O Enterprise State Roaming está disponível para todas as organizações com uma assinatura Premium do Active Directory do Azure (AD do Azure). Para obter mais detalhes sobre como obter uma assinatura do AD do Azure, consulte a página de produto do AD do Azure. |
| [Perguntas frequentes sobre configurações e roaming de dados](active-directory-windows-enterprise-state-roaming-faqs.md) | Este tópico responde a algumas dúvidas que os administradores de TI podem ter sobre as configurações e a sincronização de dados do aplicativo. |
| [Política de grupo e as configurações do MDM para a sincronização de configurações](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) | O Windows 10 fornece configurações da Política de Grupo e da política de gerenciamento de dispositivos móveis (MDM) para limitar a sincronização de configurações. |
| [Referência de configurações de roaming do Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) | Esta é uma lista completa de todas as configurações que serão ser movidas e/ou armazenadas em backup no Windows 10. |

<!---HONumber=AcomDC_0204_2016-->