---
title: Analisar logs de atividade usando logs do Azure Monitor | Microsoft Docs
description: Saiba como analisar os logs de atividade do Azure Active Directory usando os logs do Azure Monitor
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c4fde22b1b8d72ae8ae775c090e0da25ce0665f
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96181162"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs"></a>Analisar logs de atividade do Azure AD com os logs do Azure Monitor

Depois de [integrar os logs de atividades do Azure AD com os logs do Azure Monitor](howto-integrate-activity-logs-with-log-analytics.md), você pode usar o poder dos logs do Azure Monitor para obter insights sobre seu ambiente. Você também pode instalar as [exibições do Log Analytics para logs de atividade do Azure AD](howto-install-use-log-analytics-views.md) para obter acesso a relatórios pré-criados em torno de eventos de auditoria e entrada em seu ambiente.

Neste artigo, você aprenderá como analisar o logs de atividades do Azure AD no seu espaço de trabalho do Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Pré-requisitos 

Para acompanhar, você precisa:

* Um espaço de trabalho do Log Analytics em sua assinatura do Azure. Saiba como [criar um espaço de trabalho do Log Analytics](../../azure-monitor/learn/quick-create-workspace.md).
* Em primeiro lugar, conclua as etapas para [rotear os logs de atividades do Azure AD para seu espaço de trabalho do Log Analytics](howto-integrate-activity-logs-with-log-analytics.md).
*  [Acesse](../../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions) para o workspace de análise de logs
* As funções a seguir no Azure Active Directory (se você estiver acessando a análise de logs por meio do portal do Azure Active Directory)
    - Administrador de Segurança
    - Leitor de segurança
    - Leitor de relatórios
    - Administrador global
    
## <a name="navigate-to-the-log-analytics-workspace"></a>Navegar para seu espaço de trabalho do Log Analytics

1. Entre no [portal do Azure](https://portal.azure.com). 

2. Selecione **Azure Active Directory** e, em seguida, selecione **Logs** na seção **Monitoramento** para abrir o espaço de trabalho do Log Analytics. O workspace será aberto com uma consulta padrão.

    ![Consulta padrão](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Exibir o esquema para logs de atividade do Azure AD

É efetuado push dos logs para as tabelas **AuditLogs** e **SigninLogs** no workspace. Para exibir o esquema para estas tabelas:

1. Na visualização da consulta padrão na seção anterior, selecione **Esquema** e expanda o workspace. 

2. Expanda a seção **Gerenciamento de Log** e depois **AuditLogs** ou **SigninLogs** para exibir o esquema de log.
    ![Logs de auditoria](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![Logs de entrada](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Consulte os logs de atividade do Azure AD

Agora que você tem os logs em seu workspace, você pode executar consultas em relação a eles. Por exemplo, para obter os aplicativos principais usados na última semana, substitua a consulta padrão pelo seguinte e selecione **Executar**

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Para obter os principais eventos de auditoria na última semana, use a seguinte consulta:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Alerta sobre dados de log de atividades do Azure AD

Você também pode configurar alertas em sua consulta. Por exemplo, para configurar um alerta quando mais de 10 aplicativos tiverem sido usados na última semana:

1. No workspace, selecione **Definir alerta** para abrir a página **Criar regra**.

    ![Definir alerta](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Selecione os **critérios de alerta** padrão criados no alerta e atualize o **Limite** na métrica padrão para 10.

    ![Critérios do alerta](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Insira um nome e uma descrição para o alerta e escolha o nível de gravidade. Para nosso exemplo, podemos pode defini-lo como **Informativo**.

4. Selecione o **Grupo de Ações** que receberá um alerta quando ocorrer o sinal. Você pode optar por notificar a equipe por email ou mensagem de texto, ou pode automatizar a ação usando webhooks, o Azure Functions ou aplicativos lógicos. Saiba mais sobre [como criar e gerenciar grupos de alerta no portal do Azure](../../azure-monitor/platform/action-groups.md).

5. Depois de configurar o alerta, selecione **Criar alerta** para habilitá-lo. 

## <a name="use-pre-built-workbooks-for-azure-ad-activity-logs"></a>Usar pastas de trabalho predefinidas para logs de atividades do Azure AD

As pastas de trabalho fornecem vários relatórios relacionados a cenários comuns envolvendo auditoria, entrada e eventos de provisionamento. Você também pode alertar sobre qualquer um dos dados fornecidos nos relatórios seguindo as etapas descritas na seção anterior.

* **Análise de provisionamento**: esta [pasta de trabalho](../app-provisioning/application-provisioning-log-analytics.md) mostra relatórios relacionados à atividade de provisionamento de auditoria, como o número de novos usuários provisionados e falhas de provisionamento, número de usuários atualizados e falhas de atualização e o número de usuários desprovisionados e falhas correspondentes.    
* **Eventos de entradas**: esta pasta de trabalho mostra os relatórios mais relevantes relacionados à atividade de entrada de monitoramento, como entradas por aplicativo, usuário, dispositivo, bem como uma exibição de resumo que controla o número de entradas ao longo do tempo.
* **Informações de acesso condicional**: a [pasta de trabalho](../conditional-access/howto-conditional-access-insights-reporting.md) informações de acesso condicional e relatórios permite que você entenda o impacto das políticas de acesso condicional em sua organização ao longo do tempo. 

## <a name="next-steps"></a>Próximas etapas

* [Introdução às consultas de log do Azure Monitor](../../azure-monitor/log-query/get-started-queries.md)
* [Criar e gerenciar grupos de alertas no portal do Azure](../../azure-monitor/platform/action-groups.md)
* [Instalar e usar os modos de exibição do Log Analytics do Azure Active Directory](howto-install-use-log-analytics-views.md)