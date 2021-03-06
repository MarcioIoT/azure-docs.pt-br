---
title: Solução de problemas de provisionamento de nuvem Azure AD Connect
description: Este artigo descreve como solucionar problemas que podem surgir com o agente de provisionamento de nuvem.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: how-to
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fa7292d423d8b716ffd75a1a20431fb5a79bbf96
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2020
ms.locfileid: "95237333"
---
# <a name="cloud-provisioning-troubleshooting"></a>Solução de problemas de provisionamento de nuvem

O provisionamento de nuvem atinge muitas coisas diferentes e tem várias dependências diferentes. Esse escopo amplo pode dar ao aumento de vários problemas. Este artigo ajuda você a solucionar esses problemas. Ele apresenta as áreas típicas para você se concentrar, como reunir informações adicionais e as várias técnicas que você pode usar para rastrear problemas.


## <a name="common-troubleshooting-areas"></a>Áreas de solução de problemas comuns

|Name|Descrição|
|-----|-----|
|[Problemas do agente](#agent-problems)|Verifique se o agente foi instalado corretamente e se ele se comunica com o Azure Active Directory (Azure AD).|
|[Problemas de sincronização de objeto](#object-synchronization-problems)|Use os logs de provisionamento para solucionar problemas de sincronização de objetos.|
|[Provisionando problemas em quarentena](#provisioning-quarantined-problems)|Entenda o provisionamento de problemas de quarentena e como corrigi-los.|


## <a name="agent-problems"></a>Problemas do agente
Algumas das primeiras coisas que você deseja verificar com o agente são:

-  Ele está instalado?
-  O agente está em execução localmente?
-  O agente está no portal?
-  O agente está marcado como íntegro?

Esses itens podem ser verificados no portal do Azure e no servidor local que está executando o agente.

### <a name="azure-portal-agent-verification"></a>Verificação do agente de portal do Azure

Para verificar se o agente é visto pelo Azure e está íntegro, siga estas etapas.

1. Entre no portal do Azure.
1. À esquerda, selecione **Azure Active Directory** > **Azure AD Connect**. No centro, selecione **Gerenciar aprovisionamento (versão prévia)** .
1. Na tela **Provisionamento do Azure AD (versão prévia)** , selecione **Examinar todos os agentes**.

   ![Examinar todos os agentes](media/how-to-install/install-7.png)</br>
 
1. Na tela **agentes de provisionamento locais** , você vê os agentes que você instalou. Verifique se o agente em questão está lá e está marcado como *íntegro*.

   ![Tela Agentes de provisionamento local](media/how-to-install/install-8.png)</br>

### <a name="verify-the-port"></a>Verificar a porta

Verifique se o Azure está escutando na porta 443 e se o agente pode se comunicar com ele. 

Esse teste verifica se os agentes podem se comunicar com o Azure pela porta 443. Abra um navegador e vá para a URL anterior do servidor em que o agente está instalado.

![Verificação de acessibilidade de porta](media/how-to-install/verify-2.png)

### <a name="on-the-local-server"></a>No servidor local

Para verificar se o agente está em execução, siga estas etapas.

1. No servidor com o agente instalado, abra **Serviços** navegando até ele ou acessando **Iniciar**  >  **executar**  >  **Services. msc**.
1. Em **Serviços** certifique-se de que o **Atualizador do Agente do Microsoft Azure AD Connect** e o **Agente de Provisionamento do Microsoft Azure AD Connect** estão lá e se o status é *Em execução*.

   ![Tela de serviços](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-problems"></a>Problemas comuns de instalação do agente

As seções a seguir descrevem alguns problemas comuns de instalação de agente e resoluções típicas.

#### <a name="agent-failed-to-start"></a>Falha ao iniciar o agente

Você pode receber uma mensagem de erro que declara:

**Falha ao iniciar o serviço ' Microsoft Azure AD conectar o agente de provisionamento '. Verifique se você tem privilégios suficientes para iniciar os serviços do sistema.** 

Esse problema geralmente é causado por uma política de grupo que impediu que as permissões fossem aplicadas à conta de logon do serviço NT local criada pelo instalador (NT SERVICE\AADConnectProvisioningAgent). Essas permissões são necessárias para iniciar o serviço.

Para resolver esse problema, siga estas etapas.

1. Entre no servidor com uma conta de administrador.
1. Abra **Serviços** navegando até ele ou acessando **Iniciar** > **Executar** > **Services.msc**.
1. Em **Serviços**, clique duas vezes em **Microsoft Azure ad conectar agente de provisionamento**.
1. Na guia **fazer logon** , altere **essa conta** para um administrador de domínio. Em seguida, reinicie o serviço. 

   ![Guia fazer logon](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>O agente atinge o tempo limite ou o certificado é inválido

Você pode receber a seguinte mensagem de erro ao tentar registrar o agente.

![Mensagem de erro de tempo limite](media/how-to-troubleshoot/troubleshoot4.png)

Geralmente, esse problema é causado pelo agente que não consegue se conectar ao Serviço de Identidade Híbrida e exige que você configure um proxy HTTP. Para resolver esse problema, configure um proxy de saída. 

O agente de provisionamento dá suporte ao uso de um proxy de saída. Você pode configurá-lo editando o arquivo de configuração do agente *C:\Program Files\Microsoft Azure ad Connect provisionando Agent\AADConnectProvisioningAgent.exe.config*. Adicione as seguintes linhas a ela, em direção ao final do arquivo logo antes da marca de fechamento `</configuration>` .
Substitua as variáveis `[proxy-server]` e `[proxy-port]` pelo nome do servidor proxy e pelos valores de porta.

```xml
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

#### <a name="agent-registration-fails-with-security-error"></a>Falha no registro do agente com erro de segurança

Você pode receber uma mensagem de erro ao instalar o agente de provisionamento de nuvem.

Esse problema é geralmente causado pelo agente não poder executar os scripts de registro do PowerShell devido a políticas de execução do PowerShell local.

Para resolver esse problema, altere as políticas de execução do PowerShell no servidor. Você precisa ter políticas de computador e de usuário definidas como *indefinido* ou *RemoteSigned*. Se eles estiverem definidos como *irrestrito*, você verá esse erro. Para obter mais informações, consulte [políticas de execução do PowerShell](/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Arquivos de log

Por padrão, o agente emite mensagens de erro mínimas e informações de rastreamento de pilha. Você pode encontrar esses logs de rastreamento na pasta **C:\PROGRAMDATA\MICROSOFT\AZURE ad Connect Provisioning Agent\Trace**.

Para coletar detalhes adicionais para solucionar problemas relacionados ao agente, siga estas etapas.

1.  Instale o módulo do PowerShell do AADCloudSyncTools conforme descrito [aqui](reference-powershell.md#install-the-aadcloudsynctools-powershell-module).
2. Use o `Export-AADCloudSyncToolsLogs` cmdlet do PowerShell para capturar as informações.  Você pode usar as seguintes opções para ajustar sua coleta de dados.
      - SkipVerboseTrace para exportar somente os logs atuais sem capturar logs detalhados (padrão = false)
      - TracingDurationMins para especificar uma duração de captura diferente (padrão = 3 minutos)
      - OutputPath para especificar um caminho de saída diferente (padrão = documentos do usuário)


## <a name="object-synchronization-problems"></a>Problemas de sincronização de objeto

A seção a seguir contém informações sobre como solucionar problemas de sincronização de objeto.

### <a name="provisioning-logs"></a>Provisionando logs

No portal do Azure, os logs de provisionamento podem ser usados para ajudar a rastrear e solucionar problemas de sincronização de objetos. Para exibir os logs, selecione **logs**.

![Botão Logs](media/how-to-troubleshoot/log1.png)

Os logs de provisionamento fornecem uma infinidade de informações sobre o estado dos objetos que estão sendo sincronizados entre seu ambiente de Active Directory local e o Azure.

![Tela de logs de provisionamento](media/how-to-troubleshoot/log2.png)

Você pode usar as caixas suspensas na parte superior da página para filtrar a exibição para zero em problemas específicos, como datas. Clique duas vezes em um evento individual para ver informações adicionais.

![Informações da caixa suspensa de logs de provisionamento](media/how-to-troubleshoot/log3.png)

Essas informações fornecem etapas detalhadas e onde o problema de sincronização está ocorrendo. Dessa forma, você pode identificar o ponto exato do problema.


## <a name="provisioning-quarantined-problems"></a>Provisionando problemas em quarentena

O provisionamento de nuvem monitora a integridade de sua configuração e coloca objetos não íntegros em um estado de quarentena. Se a maioria ou todas as chamadas feitas no sistema de destino falharem consistentemente devido a um erro, por exemplo, credenciais de administrador inválidas, o trabalho de provisionamento será marcado como em quarentena.

![Status de quarentena](media/how-to-troubleshoot/quarantine1.png)

Ao selecionar o status, você poderá ver informações adicionais sobre a quarentena. Você também pode obter o código de erro e a mensagem.

![Informações de status de quarentena](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolve-a-quarantine"></a>Resolver uma quarentena

- Use o portal do Azure para reiniciar o trabalho de provisionamento. Na página configuração do agente, selecione **reiniciar provisionamento**.

  ![Reiniciar o provisionamento](media/how-to-troubleshoot/quarantine3.png)

- Use Microsoft Graph para [reiniciar o trabalho de provisionamento](/graph/api/synchronization-synchronizationjob-restart?tabs=http&view=graph-rest-beta). Você terá controle total sobre o que reinicia. Você pode optar por limpar:
  - Caução, para reiniciar o contador de caução acumulado para o status de quarentena.
  - Quarentena, para remover o aplicativo da quarentena.
  - Marcas d' água. 
  
  Envie a seguinte solicitação:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="repairing-the-the-cloud-sync-service-account"></a>Reparando a conta de serviço de sincronização de nuvem
Se você precisar reparar a conta de serviço de sincronização de nuvem, poderá usar o `Repair-AADCloudSyncToolsAccount` .  


   1.  Use as etapas de instalação descritas [aqui](reference-powershell.md#install-the-aadcloudsynctools-powershell-module) para começar e continue com as etapas restantes.
   2.  Em uma sessão do Windows PowerShell com privilégios administrativos, digite ou copie e cole o seguinte: 
    ```
    Connect-AADCloudSyncTools
    ```  
   3. Insira suas credenciais de administrador global do Azure AD
   4. Digite ou copie e cole o seguinte: 
    ```
    Repair-AADCloudSyncToolsAccount
    ```  
   5. Quando isso for concluído, deve-se dizer que a conta foi reparada com êxito.

## <a name="next-steps"></a>Próximas etapas 

- [O que é provisionamento?](what-is-provisioning.md)
- [O que é o provisionamento em nuvem do Azure AD Connect?](what-is-cloud-provisioning.md)
