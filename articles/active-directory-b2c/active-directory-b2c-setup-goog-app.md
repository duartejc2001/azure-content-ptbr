<properties
	pageTitle="Visualização do Active Directory B2C do Azure: configuração do Google+ | Microsoft Azure"
	description="Forneça inscrição e entrada aos consumidores com contas do Google+ em seus aplicativos protegidos pelo Azure Active Directory B2C."
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/12/2016"
	ms.author="swkrish"/>

# Preview do Azure Active Directory B2C: fornecer inscrição e conexão aos consumidores com contas do Google+

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Criar um aplicativo do Google+

Para usar o Google+ como um provedor de identidade no Azure AD (Azure Active Directory B2C), você precisará criar um aplicativo do Google+ e fornecer a ele os parâmetros certos. Para fazer isso, é necessário ter uma conta do Google+. Se não tiver uma, você poderá obtê-la em [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Acesse o [Console de Desenvolvedores do Google](https://console.developers.google.com/) e entre com suas credenciais de conta do Google+.
2. Clique em **Criar projeto**, insira um **Nome de projeto** e clique em **Criar**.

    ![Google+ – Introdução](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google+ – Novo projeto](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Clique em **Gerenciador de API** e em **Credenciais** no painel de navegação à esquerda.
4. Clique na guia **OAuth consent screen (Tela de consentimento do OAuth)** na parte superior.

    ![Google+ – Credenciais](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Selecione ou especifique um **Endereço de email** válido, forneça um **Nome de produto** e clique em **Salvar**.

    ![Google+ – Tela de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Clique em **Novas credenciais** e escolha **ID do cliente OAuth**.

    ![Google+ – Tela de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Em **Tipo de aplicativo**, selecione **Aplicativo Web**.

    ![Google+ – Tela de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Forneça um **Nome** para seu aplicativo, insira `https://login.microsoftonline.com` no campo **Origens de JavaScript autorizadas** e `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no campo **URIs de redirecionamento autorizados** campo. Substitua **{tenant}** pelo nome do locatário (por exemplo, contosob2c.onmicrosoft.com). O valor **{tenant}** diferencia letras maiúsculas de minúsculas. Clique em **Criar**.

    ![Google+ – Criar ID do cliente](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Copie os valores de **ID do cliente** e **Segredo do cliente**. Você precisará de ambos para configurar o Google+ como um provedor de identidade no seu locatário. O **Segredo do cliente** é uma credencial de segurança importante.

    ![Google+ – Segredo do cliente](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## Configurar o Google+ como um provedor de identidade no locatário

1. Siga estas etapas para [navegar até a folha de recursos do B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) no portal do Azure.
2. Na folha de recursos do B2C, clique em **Provedores de identidade**.
3. Clique em **+Adicionar**, na parte superior da folha.
4. Forneça um **Nome** amigável para a configuração do provedor de identidade. Por exemplo, insira "G+".
5. Clique em **Tipo de provedor de identidade**, selecione **Google** e clique em **OK**.
6. Clique em **Configurar este provedor de identidade** e insira a ID do cliente e o segredo do cliente do aplicativo do Google+ criado anteriormente.
7. Clique em **OK** e em **Criar** para salvar a configuração do Google+.

<!---HONumber=AcomDC_0302_2016-->