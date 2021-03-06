---
title: Registrar aplicativos da área de trabalho que chamam APIs da Web-plataforma Microsoft Identity | Azure
description: Saiba como criar um aplicativo de área de trabalho que chama APIs da Web (registro de aplicativo)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 787380dbf6f739d4b88f18a836da2146b06024c3
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443136"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Aplicativo de área de trabalho que chama APIs da Web: registro de aplicativo

Este artigo aborda as especificações de registro do aplicativo para um aplicativo de área de trabalho.

## <a name="supported-account-types"></a>Tipos de conta com suporte

Os tipos de conta com suporte em um aplicativo de área de trabalho dependem da experiência que você deseja acender. Devido a essa relação, os tipos de conta com suporte dependem dos fluxos que você deseja usar.

### <a name="audience-for-interactive-token-acquisition"></a>Público-alvo para aquisição de token interativo

Se seu aplicativo de área de trabalho usa a autenticação interativa, você pode conectar usuários de qualquer [tipo de conta](quickstart-register-app.md).

### <a name="audience-for-desktop-app-silent-flows"></a>Público-alvo dos fluxos silenciosos do aplicativo para desktop

- Para usar a autenticação integrada do Windows ou um nome de usuário e uma senha, seu aplicativo precisa conectar usuários em seu próprio locatário, por exemplo, se você for um desenvolvedor de LOB (linha de negócios). Ou, em Azure Active Directory organizações, seu aplicativo precisará conectar usuários em seu próprio locatário se for um cenário de ISV. Esses fluxos de autenticação não têm suporte para contas pessoais da Microsoft.
- Se você conectar usuários com identidades sociais que passam uma política e uma autoridade entre empresas (B2C), você só poderá usar a autenticação interativa e de nome de usuário-senha.

## <a name="redirect-uris"></a>URIs de redirecionamento

Os URIs de redirecionamento a serem usados em um aplicativo de área de trabalho dependem do fluxo que você deseja usar.

- Se você usar a autenticação interativa ou o fluxo de código do dispositivo, use `https://login.microsoftonline.com/common/oauth2/nativeclient` . Para obter essa configuração, selecione a URL correspondente na seção de **autenticação** do seu aplicativo.

  > [!IMPORTANT]
  > Hoje, o MSAL.NET usa outro URI de redirecionamento por padrão em aplicativos de área de trabalho que são executados no Windows ( `urn:ietf:wg:oauth:2.0:oob` ). No futuro, queremos alterar esse padrão, portanto, recomendamos que você use `https://login.microsoftonline.com/common/oauth2/nativeclient` .

- Se você criar um aplicativo de Objective-C ou Swift nativo para macOS, registre o URI de redirecionamento com base no identificador de pacote do seu aplicativo no seguinte formato: `msauth.<your.app.bundle.id>://auth` . Substitua `<your.app.bundle.id>` pelo identificador do pacote do seu aplicativo.
- Se seu aplicativo usar apenas a autenticação integrada do Windows ou um nome de usuário e uma senha, você não precisará registrar um URI de redirecionamento para seu aplicativo. Esses fluxos fazem uma viagem de ida e volta para o ponto de extremidade v 2.0 da plataforma de identidade da Microsoft. Seu aplicativo não será chamado de volta em nenhum URI específico.
- Para distinguir [o fluxo de código do dispositivo](scenario-desktop-acquire-token.md#device-code-flow), a [autenticação integrada do Windows](scenario-desktop-acquire-token.md#integrated-windows-authentication)e um nome de [usuário e uma senha](scenario-desktop-acquire-token.md#username-and-password) de um aplicativo cliente confidencial usando um fluxo de credencial de cliente usado em [aplicativos daemon](scenario-daemon-overview.md), nenhum deles requer um URI de redirecionamento, você precisa configurá-lo como um aplicativo cliente público. Para alcançar essa configuração:

    1. Na [portal do Azure](https://portal.azure.com), selecione seu aplicativo no **registros de aplicativo** e, em seguida, selecione **autenticação**.
    1. Em **Configurações avançadas** ,  >  **tipo de cliente padrão**  >  **tratar aplicativo como um cliente público** , selecione **Sim**.

        :::image type="content" source="media/scenarios/default-client-type.png" alt-text="Habilitar a configuração de cliente público no painel de autenticação no portal do Azure":::

## <a name="api-permissions"></a>Permissões de API

Aplicativos de desktop chamam APIs para o usuário conectado. Eles precisam solicitar permissões delegadas. Eles não podem solicitar permissões de aplicativo, que são manipuladas apenas em [aplicativos de daemon](scenario-daemon-overview.md).

## <a name="next-steps"></a>Próximas etapas

Vá para o próximo artigo neste cenário, configuração de [código do aplicativo](scenario-desktop-app-configuration.md).
