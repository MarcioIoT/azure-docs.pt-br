---
title: 'Tutorial: Integração do Azure Active Directory ao Datahug | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Datahug.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.openlocfilehash: 94cafb5044dab8e5ce6f965df73972dc21fe2744
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92454925"
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Tutorial: integração do Azure Active Directory ao Datahug

Neste tutorial, você aprenderá a integrar o Datahug ao Azure AD (Azure Active Directory).
A integração do Datahug ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem terá acesso ao Datahug.
* Você pode permitir que os usuários sejam conectados automaticamente ao Datahug (logon único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](../manage-apps/what-is-single-sign-on.md).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Prerequisites

Para configurar a integração do Azure AD ao Datahug, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Assinatura habilitada para logon único do Datahug

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Datahug dá suporte ao SSO iniciado por **SP** e **IDP**

## <a name="adding-datahug-from-the-gallery"></a>Adicionar o Datahug da galeria

Para configurar a integração do Datahug ao Azure AD, é necessário adicionar o Datahug da galeria à lista de aplicativos SaaS gerenciados.

**Para adicionar o Datahug da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory** .

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos** .

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Datahug** , selecione **Datahug** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Datahug na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Datahug, com base em um usuário de teste chamado **Brenda Fernandes** .
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Datahug.

Para configurar e testar o logon único do Azure AD com o Datahug, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Datahug](#configure-datahug-single-sign-on)** – para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Datahug](#create-datahug-test-user)** – para ter um equivalente de Brenda Fernandes no Datahug que esteja vinculado à representação de usuário do Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Datahug, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Datahug** , selecione **Logon único** .

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único** , selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML** , clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML** .

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração básica do SAML** , execute as etapas a seguir caso deseje configurar o aplicativo no modo **IDP** iniciado:

    ![Captura de tela que mostra a seção "Configuração Básica de SAML" com "Identificador", "URL de resposta" e o botão "Salvar" realçados.](common/idp-intiated.png)

    a. No **identificador** caixa de texto, digite uma URL usando o seguinte padrão: `https://apps.datahug.com/identity/<uniqueID>`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://apps.datahug.com/identity/<uniqueID>/acs`

5. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    ![Informações de logon único de Domínio e URLs do Datahug](common/metadata-upload-additional-signon.png)

    Na caixa de texto **URL de Logon** , digite uma URL: `https://apps.datahug.com/`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com o Identificador e a URL de Resposta reais. Contate a [equipe de suporte do cliente Datahug](https://www.sap.com/corporate/en/company/office-locations.html) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

6. Na página **Configurar logon único com SAML** , na seção **Certificado de autenticação SAML** , clique em **Baixar** para baixar o **XML de metadados de federação** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/metadataxml.png)

7. Na seção **Certificado de Autenticação SAML** , clique no botão **Editar** para abrir a caixa de diálogo **Certificado de Autenticação SAML** e execute as etapas a seguir.

    ![Editar o Certificado de Autenticação SAML](common/edit-certificate.png)

    a. Selecione **Assinar declaração SAML** na **Opção de Assinatura** .

    b. Selecione **SHA-1** no **Algoritmo de Assinatura** .
    
    c. Clique em **Salvar**

    ![Opção de Assinatura do Communifire](./media/datahug-tutorial/tutorial_datahug_signingoption.png)

8. Na seção **Configurar o Datahug** , copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure Ad

    c. URL de logoff

### <a name="configure-datahug-single-sign-on"></a>Configurar o logon único do Datahug

Para configurar o logon único no lado do **Datahug** , é necessário enviar o **XML de Metadados de Federação** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do Datahug](https://www.sap.com/corporate/en/company/office-locations.html). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory** , selecione **Usuários** e, em seguida, **Todos os usuários** .

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome** , insira **BrendaFernandes** .
  
    b. No campo **Nome de usuário** , digite **brendafernandes\@domíniodaempresa.extensão**  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar** .

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao Datahug.

1. No portal do Azure, selecione **Aplicativos Empresariais** , **Todos os aplicativos** e, em seguida, **Datahug** .

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Datahug** .

    ![O link do Datahug na lista Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos** .

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição** .

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos** , escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função** , escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição** , clique no botão **Atribuir** .

### <a name="create-datahug-test-user"></a>Criar um usuário de teste do Datahug

Para permitir que os usuários do Azure AD entrem no Datahug, eles precisarão ser provisionados no Datahug.  
Quando se usa o Datahug, o provisionamento é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Entre em seu site de empresa do Datahug como administrador.

2. Passe o mouse sobre a **engrenagem** no canto superior direito e clique em **Configurações**
   
    ![Captura de tela que mostra a página inicial "Datahug" com o ícone "Engrenagem" selecionado e "Configurações" selecionadas no menu suspenso.](./media/datahug-tutorial/1.png)

3. Escolha **Pessoas** e clique na guia **Adicionar Usuários**

    ![Captura de tela que mostra a página "Configurações" com a guia "Pessoas" e "Adicionar Usuários" selecionado.](./media/datahug-tutorial/2.png)

4. Digite o email da pessoa para a qual você gostaria de criar uma conta e clique em **Adicionar** .

    ![Adicionar Funcionário](./media/datahug-tutorial/3.png)

    > [!NOTE] 
    > Você pode enviar o email de registro de usuário selecionando a caixa de seleção **Enviar email de boas-vindas** .  
    > Se você estiver criando uma conta para o Salesforce, não envie o email de boas-vindas.

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Datahug no Painel de Acesso, você deverá ser conectado automaticamente ao Datahug, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](./tutorial-list.md)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)