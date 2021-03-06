---
title: Linha de base de segurança do Azure para bastiões do Azure
description: A linha de base de segurança de bastiões do Azure fornece diretrizes de procedimento e recursos para implementar as recomendações de segurança especificadas no benchmark de segurança do Azure.
author: msmbaldwin
ms.service: bastion
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 007b82b4466dfc2fbfc9c11340583175da45d92e
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025720"
---
# <a name="azure-security-baseline-for-azure-bastion"></a>Linha de base de segurança do Azure para bastiões do Azure

Essa linha de base de segurança aplica-se à orientação da [versão 2,0 do benchmark de segurança do Azure](../security/benchmarks/overview.md) para a bastiões do Azure. O Azure Security Benchmark fornece recomendações sobre como você pode proteger suas soluções de nuvem no Azure. O conteúdo é agrupado pelos **controles de segurança** definidos pelo benchmark de segurança do Azure e pelas diretrizes relacionadas aplicáveis à bastiões do Azure. Os **controles** não aplicáveis à bastiões do Azure foram excluídos.

Para ver como a bastiões do Azure é completamente mapeada para o benchmark de segurança do Azure, consulte o [arquivo completo de mapeamento de linha de base de segurança de bastiões do Azure](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Segurança de rede

*Para obter mais informações, consulte o [benchmark de segurança do Azure: segurança de rede](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementar a segurança para tráfego interno

**Orientação**: ao implantar recursos de bastiões do Azure, você deve criar ou usar uma rede virtual existente. Verifique se todas as redes virtuais do Azure seguem um princípio de segmentação empresarial que se alinha aos riscos de negócios. Qualquer sistema que possa incorrer em um risco maior para a organização deve ser isolado em sua própria rede virtual e suficientemente protegido com um NSG (grupo de segurança de rede).

O serviço de bastiões do Azure requer que as seguintes portas precisem ser abertas para que o serviço funcione corretamente:

- Tráfego de entrada:
   - Tráfego de entrada da Internet pública: a bastiões do Azure criará um IP público que precisa da porta 443 habilitada no IP público para o tráfego de entrada. A porta 3389/22 não precisa ser aberta no AzureBastionSubnet. 

   - Tráfego de entrada do plano de controle de bastiões do Azure: para conectividade do plano de controle, habilite a porta 443 de entrada da marca de serviço do Gatewaymanager. Isso permite que o plano de controle, ou seja, o Gerenciador de gateway possa se comunicar com a bastiões do Azure.

- Tráfego de saída:

   - Tráfego de saída para VMs (máquinas virtuais) de destino: a bastiões do Azure alcançará as VMs de destino por IP privado. O NSGs precisa permitir o tráfego de saída para outras sub-redes de VM de destino para a porta 3389 e 22.

   - Tráfego de saída para outros pontos de extremidade públicos no Azure: a bastiões do Azure precisa ser capaz de se conectar a vários pontos de extremidade públicos no Azure (por exemplo, para armazenar logs de diagnóstico e logs de medição). Por esse motivo, a bastiões do Azure precisa de saída para 443 para a marca de serviço AzureCloud.

A conectividade com o Gerenciador de gateway e a marca de serviço do Azure é protegida (bloqueada) pelos certificados do Azure. Entidades externas, incluindo os consumidores desses recursos, não podem se comunicar nesses pontos de extremidade. 

- [Como criar um grupo de segurança de rede com regras de segurança](../virtual-network/tutorial-filter-network-traffic.md)

- [Você pode saber mais sobre o requisito de NSG de bastiões aqui](bastion-nsg.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="ns-2-connect-private-networks-together"></a>NS-2: conectar redes privadas juntas

**Diretrizes**: a bastiões do Azure não expõe nenhum ponto de extremidade que possa ser acessado por meio de uma rede privada. A bastiões do Azure dá suporte à implantação em uma rede emparelhada para centralizar sua implantação de bastiões e habilitar a conectividade entre redes.

- [Emparelhamento de rede virtual e bastiões do Azure](vnet-peering.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="identity-management"></a>Gerenciamento de Identidades

*Para obter mais informações, consulte o [benchmark de segurança do Azure: gerenciamento de identidades](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: padronizar Azure Active Directory como o sistema de identidade e autenticação central

**Diretrizes**: a bastiões do Azure é integrada ao Azure Active Directory (Azure AD), que é o serviço de gerenciamento de identidade e acesso padrão do Azure. Os usuários podem acessar o portal do Azure usando a autenticação do Azure AD para gerenciar o serviço de bastiões do Azure (criar, atualizar e excluir recursos de bastiões).

Conectar-se a máquinas virtuais usando a bastiões do Azure depende de uma chave SSH ou nome de usuário/senha e atualmente não dá suporte ao uso de credenciais do Azure AD.

Além de uma chave SSH ou nome de usuário/senha, ao se conectar a máquinas virtuais usando a bastiões do Azure, seu usuário precisará das seguintes atribuições de função:
- Função de leitor na máquina virtual de destino
- Função de leitor na NIC com o IP privado da máquina virtual de destino
- Função de leitor no recurso do Azure Bastion

Para saber mais, consulte as referências a seguir:

- [Conectar-se a uma máquina virtual do Linux usando a bastiões do Azure](bastion-connect-vm-ssh.md)

- [Conectar-se a uma máquina virtual do Windows usando a bastiões do Azure](bastion-connect-vm-rdp.md)

- [Funções internas do Azure](../role-based-access-control/built-in-roles.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: usar o SSO (logon único) do Azure AD para acesso ao aplicativo

**Diretrizes**: a bastiões do Azure não dá suporte a SSO para autenticação ao autenticar para recursos de máquina virtual, somente SSH ou nome de usuário/senha têm suporte. No entanto, a bastiões do Azure usa o Azure Active Directory (Azure AD) para fornecer gerenciamento de acesso e identidade para o serviço geral. Os usuários podem se autenticar no Azure AD para acessar e gerenciar seus recursos de bastiões do Azure e experimentar o logon único contínuo com suas próprias identidades corporativas sincronizadas por meio de Azure AD Connect. 

- [Entender o SSO do aplicativo com o Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Azure AD Connect](../active-directory/hybrid/whatis-azure-ad-connect.md)

- [Conectar-se a uma máquina virtual do Linux usando a bastiões do Azure](bastion-connect-vm-ssh.md)

- [Conectar-se a uma máquina virtual do Windows usando a bastiões do Azure](bastion-connect-vm-rdp.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: usar controles de autenticação forte para todo o acesso baseado em Azure Active Directory

**Diretrizes**: a bastiões do Azure é integrada ao Azure Active Directory (Azure AD) para acesso e gerenciamento do serviço. Configure a autenticação multifator do Azure para seu locatário do Azure AD. O Azure AD dá suporte a controles de autenticação fortes por meio da MFA (autenticação multifator) e de métodos fortes de senha.  
- Autenticação multifator: habilite o Azure AD MFA e siga as recomendações de gerenciamento de acesso e identidade da central de segurança do Azure para sua configuração de MFA. A MFA pode ser imposta em todos os usuários, Selecionar usuários ou no nível por usuário com base nas condições de entrada e nos fatores de risco. 

- Autenticação com senha: três opções de autenticação com senha estão disponíveis: Windows Hello para empresas, Microsoft Authenticator app e métodos de autenticação locais, como cartões inteligentes. 

- [Como habilitar a MFA no Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Introdução a opções de autenticação com senha para Azure Active Directory](../active-directory/authentication/concept-authentication-passwordless.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: monitorar e alertar em anomalias de conta

**Orientação**: habilite os logs de diagnóstico para sessões remotas de bastiões do Azure e use esses logs para exibir quais usuários se conectaram a quais cargas de trabalho, em que hora, de onde e outras informações de registro em log relevantes. Crie alertas para determinadas sessões de bastiões registradas usando Azure Monitor ser notificado quando houver anomalias detectadas nos logs.

- [Você pode encontrar mais informações sobre como habilitar o log de diagnóstico aqui](diagnostic-logs.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: restringir o acesso a recursos do Azure com base em condições

**Orientação**: você só pode acessar o serviço de bastiões do Azure por meio do portal do Azure, o acesso ao portal do Azure pode ser restringido usando o acesso condicional do Azure Active Directory (AD do Azure). Use o acesso condicional do Azure AD para obter um controle de acesso mais granular com base em condições definidas pelo usuário, como exigir logons de usuário de determinados intervalos de IP para usar a MFA.

O cliente também pode usar diferentes políticas de controle de acesso baseado em função em nível de máquinas virtuais ingressadas no domínio para restringir ainda mais o acesso à máquina virtual.

- [Você pode ler mais sobre o acesso condicional do Azure AD aqui](../active-directory/conditional-access/overview.md)

- [Visão geral do acesso condicional do Azure](../active-directory/conditional-access/overview.md)

- [Políticas de acesso condicional comum](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Configurar o gerenciamento de sessão de autenticação com acesso condicional](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="privileged-access"></a>Acesso privilegiado

*Para obter mais informações, consulte o [benchmark de segurança do Azure: acesso privilegiado](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: restringir o acesso administrativo a sistemas críticos para os negócios

**Diretrizes**: a bastiões do Azure usa o Azure RBAC (controle de acesso baseado em função) para isolar o acesso a sistemas críticos de negócios, restringindo quais contas têm acesso concedido para se conectar a determinadas máquinas virtuais. Certifique-se de seguir o princípio de privilégios mínimos para que os usuários tenham apenas as permissões necessárias para executar suas tarefas específicas.

Certifique-se de também restringir o acesso aos sistemas de gerenciamento, identidade e segurança que têm acesso administrativo ao seu acesso comercialmente crítico, como controladores de Domínio do Active Directory (DCs), ferramentas de segurança e ferramentas de gerenciamento do sistema com agentes instalados em sistemas comerciais críticos. Os invasores que comprometem esses sistemas de gerenciamento e segurança podem fazê-los imediatamente para comprometer os ativos críticos aos negócios.

Todos os tipos de controles de acesso devem ser alinhados à sua estratégia de segmentação corporativa para garantir o controle de acesso consistente.

- [Funções necessárias para acessar uma máquina virtual com a bastiões do Azure](bastion-faq.md#roles)

- [Componentes do Azure e modelo de referência](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Acesso ao grupo de gerenciamento](../governance/management-groups/overview.md#management-group-access)

- [Administradores de assinatura do Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: revisar e reconciliar o acesso do usuário regularmente

**Diretrizes**: a bastiões do Azure usa contas do Azure Active Directory (Azure AD) e o RBAC do Azure para gerenciar seus recursos. Revise as contas de usuário e a atribuição de acesso regularmente para garantir que as contas e seu acesso sejam válidos. Você pode usar as revisões de acesso do Azure AD para revisar as associações de grupo, o acesso aos aplicativos empresariais e as atribuições de função. Os relatórios do AD do Azure podem fornecer logs para ajudar a descobrir contas obsoletas. Você também pode usar Azure AD Privileged Identity Management para criar o fluxo de trabalho de relatório de revisão de acesso para facilitar o processo de revisão.

Além disso, o Azure Privileged Identity Management também pode ser configurado para alertar quando um número excessivo de contas de administrador for criado e para identificar as contas de administrador que estão obsoletas ou configuradas incorretamente.

- [Criar uma revisão de acesso das funções de recurso do Azure no Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Removendo o acesso a uma delegação](../lighthouse/how-to/remove-delegation.md)

- [Como usar as revisões de identidade e acesso do Azure AD](../active-directory/governance/access-reviews-overview.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: configurar o acesso de emergência no Azure AD

**Diretrizes**: a bastiões do Azure é integrada ao Azure Active Directory e ao RBAC do Azure para gerenciar seus recursos. Para evitar que seja bloqueado acidentalmente da sua organização do Azure AD, configure uma conta de acesso de emergência para acesso quando contas administrativas normais não puderem ser usadas. As contas de acesso de emergência geralmente são altamente privilegiadas e não devem ser atribuídas a indivíduos específicos. As contas de acesso de emergência são limitadas à emergência ou cenários de urgência em que as contas administrativas normais não podem ser usadas.

Você deve garantir que as credenciais (como senha, certificado ou cartão inteligente) para contas de acesso de emergência sejam mantidas seguras e conhecidas apenas para indivíduos que estão autorizados a usá-las somente em uma emergência.

- [Gerenciar contas de acesso de emergência no Microsoft Azure Active Directory](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="pa-5-automate-entitlement-management"></a>PA-5: automatizar o gerenciamento de direitos 

**Diretrizes**: a bastiões do Azure é integrada ao Azure Active Directory (Azure AD) e ao RBAC do Azure para gerenciar seus recursos. Use os recursos de gerenciamento de direitos do Azure AD para automatizar fluxos de trabalho de solicitação de acesso, incluindo atribuições de acesso, revisões e expiração. Também há suporte para a aprovação dupla ou de vários estágios.

- [O que são revisões de acesso do Azure AD](../active-directory/governance/access-reviews-overview.md)

- [O que é o gerenciamento de direitos do AD do Azure](../active-directory/governance/entitlement-management-overview.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: usar estações de trabalho com acesso privilegiado

**Diretrizes**: estações de trabalho seguras e isoladas são extremamente importantes para a segurança de funções confidenciais, como administradores, desenvolvedores e operadores de serviço críticos. Dependendo dos seus requisitos, você pode usar estações de trabalho de usuário altamente protegidas para executar tarefas de gerenciamento administrativo com seus recursos de bastiões do Azure em ambientes de produção. Use Azure Active Directory, a ATP (proteção avançada contra ameaças) do Microsoft defender e/ou Microsoft Intune para implantar uma estação de trabalho de usuário segura e gerenciada para tarefas administrativas. As estações de trabalho protegidas podem ser gerenciadas centralmente para impor configuração segura, incluindo autenticação forte, linhas de base de software e hardware e acesso lógico restrito à rede. 

- [Entender as estações de trabalho com acesso privilegiado](../active-directory/devices/concept-azure-managed-workstation.md)

- [Implantar uma estação de trabalho com acesso privilegiado](../active-directory/devices/howto-azure-managed-workstation.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: seguir apenas administração suficiente (princípio de privilégio mínimo) 

**Diretrizes**: a bastiões do Azure é integrada ao RBAC (controle de acesso baseado em função) do Azure para gerenciar seus recursos. O RBAC do Azure permite gerenciar o acesso a recursos do Azure por meio de atribuições de função. Você pode atribuir essas funções internas a usuários, grupos, entidades de serviço e identidades gerenciadas. Há funções internas predefinidas para determinados recursos, e essas funções podem ser inventariadas ou consultadas por meio de ferramentas como CLI do Azure, Azure PowerShell ou portal do Azure. Os privilégios que você atribui aos recursos por meio do RBAC do Azure devem ser sempre limitados ao que é exigido pelas funções. Isso complementa a abordagem JIT (just in time) do Azure AD Privileged Identity Management (PIM) e deve ser revisado periodicamente. Use funções internas para alocar permissão e apenas para criar funções personalizadas quando necessário.

Ao se conectar a máquinas virtuais usando a bastiões do Azure, seu usuário precisará das seguintes atribuições de função:
- Função de leitor na máquina virtual de destino
- Função de leitor na NIC com o IP privado da máquina virtual de destino
- Função de leitor no recurso do Azure Bastion

Para saber mais, consulte as referências a seguir:

- [Conectar-se a uma máquina virtual do Linux usando a bastiões do Azure](bastion-connect-vm-ssh.md)

- [Conectar-se a uma máquina virtual do Windows usando a bastiões do Azure](bastion-connect-vm-rdp.md)

- [Funções internas do Azure](../role-based-access-control/built-in-roles.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="asset-management"></a>Gerenciamento de Ativos

*Para obter mais informações, consulte o [benchmark de segurança do Azure: gerenciamento de ativos](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: garantir que a equipe de segurança tenha visibilidade dos riscos dos ativos

**Orientação**: garanta que as equipes de segurança recebam permissões de leitor de segurança em seu locatário e assinaturas do Azure para que possam monitorar riscos de segurança usando a central de segurança do Azure. 

Dependendo de como as responsabilidades da equipe de segurança são estruturadas, o monitoramento de riscos de segurança pode ser a responsabilidade de uma equipe de segurança central ou de uma equipe local. Dito isso, as informações e os riscos de segurança sempre devem ser agregados centralmente dentro de uma organização. 

As permissões de leitor de segurança podem ser aplicadas amplamente a um locatário inteiro (grupo de gerenciamento raiz) ou com escopo para grupos de gerenciamento ou assinaturas específicas. 

Observação: podem ser necessárias permissões adicionais para obter visibilidade em cargas de trabalho e serviços. 

- [Visão geral da função de leitor de segurança](../role-based-access-control/built-in-roles.md#security-reader)

- [Visão geral do Azure Grupos de Gerenciamento](../governance/management-groups/overview.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: garantir que a equipe de segurança tenha acesso ao inventário de ativos e metadados

**Diretrizes**: aplique marcas aos recursos de bastiões, grupos de recursos e assinaturas do Azure para organizá-los logicamente em uma taxonomia. Cada marca consiste em um par de nome/valor. Por exemplo, você pode aplicar o nome "Ambiente" e o valor "Produção" a todos os recursos na produção. Garanta que as equipes de segurança tenham acesso a um inventário de ativos continuamente atualizado no Azure. Eles podem usar o grafo de recursos do Azure para consultar e descobrir todos os recursos em suas assinaturas, incluindo serviços do Azure, aplicativos e recursos de rede.

- [Como criar consultas com o Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Gerenciamento de inventário de ativos da central de segurança do Azure](../security-center/asset-inventory.md)

- [Para obter mais informações sobre como marcar ativos, consulte o guia de decisão de nomenclatura e marcação de recursos](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: usar apenas os serviços aprovados do Azure

**Diretrizes**: Use Azure Policy para auditar e restringir quais serviços os usuários podem provisionar em seu ambiente, isso inclui a capacidade de permitir ou negar implantações de recursos de bastiões do Azure. Use o grafo de recursos do Azure para consultar e descobrir recursos em suas assinaturas. Você também pode usar Azure Monitor para criar regras para disparar alertas quando um serviço não aprovado for detectado.

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Como negar um tipo de recurso específico com o Azure Policy](../governance/policy/samples/built-in-policies.md#general)

- [Como criar consultas com o Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: garantir a segurança do gerenciamento do ciclo de vida do ativo

**Diretrizes**: Remova o acesso aos recursos de bastiões do Azure que foram implantados quando não forem mais necessários para minimizar a superfície de ataque. Os usuários podem gerenciar seus recursos de bastiões do Azure por meio do portal do Azure, da CLI ou de APIs REST. Você também pode excluir ou forçar a desconexão de uma sessão remota contínua se ela não for mais necessária ou identificada como uma ameaça potencial.

- [Excluir de forçar a desconexão de uma sessão remota](session-monitoring.md#view)

- [CLI de rede do Azure](https://docs.microsoft.com/powershell/module/az.network/?view=azps-5.1.0#networking&amp;preserve-view=true)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: limitar a capacidade dos usuários de interagir com Azure Resource Manager

**Diretrizes**: a bastiões do Azure é integrada ao Azure Active Directory (Azure AD) para identidade e autenticação. Você pode usar o acesso condicional do Azure para limitar a capacidade dos usuários de interagir com o Gerenciador de recursos do Azure Configurando "bloquear acesso" para o aplicativo de "gerenciamento de Microsoft Azure".

- [Como configurar o acesso condicional para bloquear o acesso ao Gerenciador de recursos do Azure](../role-based-access-control/conditional-access-azure-management.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="logging-and-threat-detection"></a>Registro em log e detecção de ameaças

*Para obter mais informações, consulte o [benchmark de segurança do Azure: registro em log e detecção de ameaças](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: habilitar a detecção de ameaças para gerenciamento de identidade e acesso do Azure

**Diretrizes**: a bastiões do Azure se integra ao Azure Active Directory (Azure AD) e o serviço é acessado pela portal do Azure. Por padrão, as ações de gerenciamento para o serviço (como criar, atualizar e excluir) são capturadas por meio do log de atividades do Azure. Os usuários também devem habilitar logs de recursos de bastiões do Azure, como BastionAuditLogs de sessão para rastrear sessões de bastiões.

O Azure AD fornece os seguintes logs de usuário que podem ser exibidos no relatório do Azure AD ou integrados com o Azure Monitor, o Azure Sentinel ou outras ferramentas de monitoramento/SIEM para casos de uso de monitoramento e análise mais sofisticados: 
-   Entradas – O relatório de entradas fornece informações sobre o uso de aplicativos gerenciados e atividades de entrada do usuário.

-   Logs de auditoria – Permitem o rastreio de todas as alterações feitas por vários recursos no Azure AD por meio de logs. Exemplos de logs de auditoria incluem alterações feitas em quaisquer recursos no Azure AD, como adicionar ou remover usuários, aplicativos, grupos, funções e políticas.

-   Entradas arriscadas - uma entrada arriscada é um indicador para uma tentativa de logon que pode ter sido realizada por alguém que não é o proprietário legítimo de uma conta de usuário.

-   Usuários sinalizados para riscos - um usuário arriscado é um indicador de uma conta de usuário que pode ter sido comprometida.

A central de segurança do Azure também pode alertar sobre determinadas atividades suspeitas, como um número excessivo de tentativas de autenticação com falha e contas preteridas na assinatura. Além do monitoramento básico de higiene de segurança, o módulo de proteção contra ameaças da central de segurança do Azure também pode coletar alertas de segurança mais aprofundados de recursos de computação individuais do Azure (como máquinas virtuais, contêineres, serviço de aplicativo), recursos de dados (como banco de dado SQL e armazenamento) e camadas de serviço do Azure. Essa funcionalidade permite que você veja anomalias de conta dentro dos recursos individuais.

- [Logs de recursos de bastiões do Azure](diagnostic-logs.md)

- [Relatórios de atividade de auditoria no Azure AD](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Habilitar a proteção de identidade do Azure](../active-directory/identity-protection/overview-identity-protection.md)

- [Proteção contra ameaças na Central de Segurança do Azure](/azure/security-center/threat-protection)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: habilitar o registro em log para atividades de rede do Azure

**Orientação**: Habilitar logs de recursos de bastiões do Azure, use esses logs de diagnóstico para exibir quais usuários se conectaram a quais cargas de trabalho, em que hora, de onde e outras informações de log relevantes. Os usuários podem configurar esses logs para serem enviados a uma conta de armazenamento para a retenção e a auditoria de longo prazo.

Habilite e colete logs de recursos do NSG (grupo de segurança de rede) e logs de fluxo do NSG nos grupos de segurança de rede que são aplicados às redes virtuais que você tem o recurso de bastiões do Azure implantado. Esses logs podem então ser usados para analisar a segurança de rede e para dar suporte a investigações de incidentes, busca de ameaças e geração de alertas de segurança. Você pode enviar os logs de fluxo para um espaço de trabalho Azure Monitor Log Analytics e, em seguida, usar Análise de Tráfego para fornecer informações.

- [Habilitar e trabalhar com os logs de bastiões do Azure](diagnostic-logs.md)

- [Como habilitar logs de fluxo do grupo de segurança de rede](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Logs e métricas do Firewall do Azure](../firewall/logs-and-metrics.md)

- [Como habilitar e usar Análise de Tráfego](../network-watcher/traffic-analytics.md)

- [Monitoramento com o observador de rede](../network-watcher/network-watcher-monitoring-overview.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: habilitar o registro em log para recursos do Azure

**Diretrizes**: logs de atividade, que estão disponíveis automaticamente, contêm todas as operações de gravação (put, post, Delete) para seus recursos de bastiões do Azure, exceto operações de leitura (Get). Os logs de atividades podem ser usados para encontrar um erro ao solucionar problemas ou para monitorar como um usuário em sua organização modificou um recurso.

- [Como coletar logs e métricas de plataforma com Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Entender o registro em log e diferentes tipos de log no Azure](../azure-monitor/platform/platform-logs-overview.md)

- [Habilitar logs de recursos do Azure para bastiões do Azure ](diagnostic-logs.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: centralizar o gerenciamento e a análise do log de segurança

**Orientação**: centralizar o armazenamento e a análise de log para habilitar a correlação. Para cada origem de log, verifique se você atribuiu um proprietário de dados, orientação de acesso, local de armazenamento, quais ferramentas são usadas para processar e acessar os dados e os requisitos de retenção de dados.

Verifique se você está integrando os logs de atividades do Azure ao seu registro central. Os logs de ingestão por meio de Azure Monitor para agregar dados de segurança gerados por dispositivos de ponto de extremidade, recursos de rede e outros sistemas de segurança. No Azure Monitor, use espaços de trabalho do Log Analytics para consultar e executar análises e usar contas de armazenamento do Azure para armazenamento de longo prazo e arquivamento.

Além disso, habilite e integre dados ao Azure Sentinel ou a um SIEM de terceiros.

Muitas organizações optam por usar o Azure Sentinel para dados "quentes" que são usados com frequência e o armazenamento do Azure para dados "frios" que são usados com menos frequência.

- [Como coletar logs e métricas de plataforma com Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Como integrar o Azure Sentinel](../sentinel/quickstart-onboard.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: configurar a retenção de armazenamento de log

**Orientação**: Verifique se as contas de armazenamento ou os espaços de trabalho log Analytics usados para armazenar os logs de bastiões do Azure têm o período de retenção de log definido de acordo com os regulamentos de conformidade da sua organização.

No Azure Monitor, você pode definir seu período de retenção de espaço de trabalho de Log Analytics de acordo com os regulamentos de conformidade de sua organização.

- [Como configurar Log Analytics período de retenção do espaço de trabalho](../azure-monitor/platform/manage-cost-storage.md)

- [Armazenando logs de recursos em uma conta de armazenamento do Azure](/azure/azure-monitor/platform/resource-logs-collect-storage)

- [Habilitar e trabalhar com os logs de bastiões do Azure](diagnostic-logs.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

## <a name="incident-response"></a>Resposta a incidentes

*Para obter mais informações, consulte o [benchmark de segurança do Azure: resposta a incidentes](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: preparação – processo de atualização de resposta a incidentes para o Azure

**Orientação**: Verifique se sua organização tem processos para responder a incidentes de segurança, atualizou esses processos para o Azure e está experimentando-os regularmente para garantir a preparação.

- [Implementar a segurança em todo o ambiente corporativo](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guia de referência de resposta a incidentes](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: preparação – notificação de incidente de instalação

**Orientação**: configurar informações de contato de incidente de segurança na central de segurança do Azure. Essas informações de contato são usadas pela Microsoft para entrar em contato com você se o MSRC (Microsoft Security Response Center) descobre que seus dados foram acessados por uma parte ilegal ou não autorizada. Você também tem opções para personalizar o alerta de incidente e a notificação em diferentes serviços do Azure com base em suas necessidades de resposta a incidentes. 

- [Como definir o contato da segurança da central de segurança do Azure](../security-center/security-center-provide-security-contact-details.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: detecção e análise – crie incidentes com base em alertas de alta qualidade

**Diretrizes**: Verifique se você tem um processo para criar alertas de alta qualidade e medir a qualidade dos alertas. Isso permite que você aprenda as lições dos últimos incidentes e Priorize os alertas para analistas, para que eles não percam tempo em falsos positivos.

Alertas de alta qualidade podem ser criados com base na experiência dos últimos incidentes, fontes de comunidade validadas e ferramentas projetadas para gerar e limpar alertas, combinando e correlacionando diferentes fontes de sinal. 

A central de segurança do Azure fornece alertas de alta qualidade em vários ativos do Azure. Você pode usar o conector de dados ASC para transmitir os alertas para o Azure Sentinel. O Azure Sentinel permite que você crie regras de alerta avançadas para gerar incidentes automaticamente para uma investigação. 

Exporte seus alertas e recomendações da central de segurança do Azure usando o recurso exportar para ajudar a identificar os riscos para os recursos do Azure. Exporte alertas e recomendações de forma manual ou contínua, de modo contínuo.

- [Como configurar a exportação](../security-center/continuous-export.md)

- [Como transmitir alertas para o Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: detecção e análise – investigue um incidente

**Orientação**: garanta que os analistas possam consultar e usar diversas fontes de dados à medida que investigam incidentes potenciais, para criar uma visão completa do que aconteceu. Logs diversos devem ser coletados para acompanhar as atividades de um invasor potencial na cadeia de Kill para evitar manchas cegas.  Você também deve garantir que informações e aprendizados sejam capturados para outros analistas e para futura referência histórica.  

As fontes de dados para investigação incluem as fontes de registro em log centralizadas que já estão sendo coletadas dos serviços dentro do escopo e sistemas em execução, mas também podem incluir:

- Dados de rede – Use os logs de fluxo dos grupos de segurança de rede, o observador de rede do Azure e o Azure Monitor para capturar logs de fluxo de rede e outras informações de análise. 

- Instantâneos de sistemas em execução: 

    - Use a capacidade de instantâneo da máquina virtual do Azure para criar um instantâneo do disco do sistema em execução. 

    - Use o recurso de despejo de memória nativo do sistema operacional para criar um instantâneo da memória do sistema em execução.

    - Use o recurso de instantâneo dos serviços do Azure ou a capacidade de seu próprio software para criar instantâneos dos sistemas em execução.

O Azure Sentinel fornece ampla análise de dados em praticamente qualquer origem de log e um portal de gerenciamento de casos para gerenciar o ciclo de vida completo de incidentes. As informações de inteligência durante uma investigação podem ser associadas a um incidente para fins de rastreamento e relatório. 

- [Instantâneo do disco de um computador Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Instantâneo do disco de um computador Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Suporte Microsoft Azure informações de diagnóstico e coleta de despejo de memória](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Investigue incidentes com o Azure Sentinel](../sentinel/tutorial-investigate-cases.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: detecção e análise – priorizar incidentes

**Diretrizes**: forneça o contexto para analistas nos quais os incidentes se concentram primeiro com base na severidade do alerta e na sensibilidade do ativo. 

A central de segurança do Azure atribui uma severidade a cada alerta para ajudá-lo a priorizar quais alertas devem ser investigados primeiro. A gravidade se baseia em quão confiante a central de segurança está na localização ou na análise usada para emitir o alerta, bem como o nível de confiança de que houve uma intenção mal-intencionada por trás da atividade que levou ao alerta.

Além disso, marque os recursos usando marcas e crie um sistema de nomeação para identificar e categorizar os recursos do Azure, especialmente aqueles que processam dados confidenciais.  É sua responsabilidade priorizar a correção de alertas com base na criticalidade dos recursos do Azure e do ambiente em que o incidente ocorreu.

- [Alertas na Central de Segurança do Azure](../security-center/security-center-alerts-overview.md)

- [Usar marcas para organizar seus recursos do Azure](/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: contenção, erradicação e recuperação – automatizar o tratamento de incidentes

**Diretrizes**: automatize tarefas repetitivas manuais para acelerar o tempo de resposta e reduzir a carga de analistas. As tarefas manuais demoram mais para serem executadas, atrasando cada incidente e reduzindo Quantos incidentes um analista pode manipular. As tarefas manuais também aumentam a fadiga do analista, o que aumenta o risco de erros humanos que causam atrasos e degrada a capacidade dos analistas de se concentrarem efetivamente em tarefas complexas. Use os recursos de automação de fluxo de trabalho na central de segurança do Azure e o Azure Sentinel para disparar automaticamente ações ou executar um guia estratégico para responder a alertas de segurança de entrada. O guia estratégico executa ações, como enviar notificações, desabilitar contas e isolar redes problemáticas. 

- [Configurar a automação do fluxo de trabalho na central de segurança](../security-center/workflow-automation.md)

- [Configurar respostas de ameaças automatizadas na central de segurança do Azure](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Configurar respostas de ameaças automatizadas no Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

## <a name="posture-and-vulnerability-management"></a>Postura e gerenciamento de vulnerabilidades

*Para obter mais informações, consulte o [benchmark de segurança do Azure: postura e gerenciamento de vulnerabilidade](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1: estabelecer configurações seguras para os serviços do Azure 

**Orientação**: definir e implementar configurações de segurança padrão para a bastiões do Azure com o Azure Policy. Use Azure Policy aliases no namespace "Microsoft. Network" para criar políticas personalizadas para auditar ou impor a configuração de rede de sua bastiões do Azure. Os clientes também podem estabelecer configurações seguras aproveitando plantas do Azure ou modelos de ARM para implantar recursos de bastiões de forma segura e consistente.

- [Como exibir os aliases disponíveis do Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Saiba mais sobre modelos do ARM](../azure-resource-manager/templates/overview.md)

- [Visão geral sobre os planos gráficos do Azure](../governance/blueprints/overview.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2: sustentar configurações seguras para os serviços do Azure

**Orientação**: definir e implementar configurações de segurança padrão para a bastiões do Azure com o Azure Policy. Use Azure Policy aliases no namespace "Microsoft. Network" para criar políticas personalizadas para auditar ou impor a configuração de rede de seus recursos de bastiões.

- [Como exibir os aliases disponíveis do Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: conduzir simulação de ataque regular

**Orientação**: conforme necessário, realize testes de penetração ou atividades de equipe vermelhas em seus recursos do Azure e garanta a correção de todas as descobertas de segurança críticas.

Siga as regras de teste de penetração Microsoft Cloud do Engagement para garantir que seus testes de penetração não estejam violando as políticas da Microsoft. Use a estratégia da Microsoft e a execução de equipes vermelhas e testes de penetração de sites ativos em infraestrutura de nuvem, serviços e aplicativos gerenciados pela Microsoft.

- [Teste de penetração no Azure](../security/fundamentals/pen-testing.md)

- [Regras de participação para testes de penetração](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud o agrupamento vermelho](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

## <a name="governance-and-strategy"></a>Governança e estratégia

*Para obter mais informações, consulte o [benchmark de segurança do Azure: governança e estratégia](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: definir a estratégia de gerenciamento de ativos e de proteção de dados 

**Orientação**: Certifique-se de documentar e comunicar uma estratégia clara para monitoramento contínuo e proteção de sistemas e dados. Priorize a descoberta, a avaliação, a proteção e o monitoramento de sistemas e dados críticos para os negócios. 

Essa estratégia deve incluir diretrizes documentadas, políticas e padrões para os seguintes elementos: 

-   Padrão de classificação de dados de acordo com os riscos de negócios

-   Visibilidade da organização de segurança em riscos e inventário de ativos 

-   Aprovação da organização de segurança dos serviços do Azure para uso 

-   Segurança de ativos por meio de seu ciclo de vida

-   Estratégia de controle de acesso necessária de acordo com a classificação de dados organizacionais

-   Uso de recursos de proteção de dados nativos e de terceiros do Azure

-   Requisitos de criptografia de dados para casos de uso em trânsito e em repouso

-   Padrões de criptografia apropriados

Para saber mais, consulte as referências a seguir:
- [Recomendação da arquitetura de segurança do Azure – armazenamento, dados e criptografia](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Conceitos básicos de segurança do Azure-segurança de dados do Azure, criptografia e armazenamento](../security/fundamentals/encryption-overview.md)

- [Estrutura de adoção de nuvem – práticas recomendadas de segurança de dados do Azure e criptografia](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Benchmark de segurança do Azure – gerenciamento de ativos](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Benchmark de segurança do Azure-proteção de dados](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: definir estratégia de segmentação corporativa 

**Orientação**: estabeleça uma estratégia empresarial para segmentar o acesso aos ativos usando uma combinação de identidade, rede, aplicativo, assinatura, grupo de gerenciamento e outros controles.

Equilibre cuidadosamente a necessidade de separação de segurança com a necessidade de habilitar a operação diária dos sistemas que precisam se comunicar entre si e acessar dados.

Certifique-se de que a estratégia de segmentação seja implementada consistentemente nos tipos de controle, incluindo segurança de rede, modelos de identidade e acesso, além de permissões de aplicativo/modelos de acesso e controles de processos humanos.

- [Diretrizes sobre a estratégia de segmentação no Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Diretrizes sobre a estratégia de segmentação no Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alinhar a segmentação de rede com a estratégia de segmentação corporativa](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: definir estratégia de gerenciamento de postura de segurança

**Diretrizes**: meça e reduza continuamente os riscos para seus ativos individuais e o ambiente no qual eles estão hospedados. Priorize os ativos de alto valor e as superfícies de ataque altamente expostas, como aplicativos publicados, pontos de entrada e saída de rede, os pontos de extremidade de usuário e administrador, etc.

- [Benchmark de segurança do Azure – gerenciamento de postura e vulnerabilidade](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: alinhar funções de organização, responsabilidades e responsabilidades

**Orientação**: Certifique-se de documentar e comunicar uma estratégia clara para funções e responsabilidades em sua organização de segurança. Priorize proporcionando responsabilidade clara por decisões de segurança, educando todos no modelo de responsabilidade compartilhada e instruir equipes técnicas sobre tecnologia para proteger a nuvem.

- [Prática recomendada de segurança do Azure 1 – pessoas: treinar equipes na jornada de segurança de nuvem](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Prática recomendada de segurança do Azure 2-pessoas: educar equipes sobre tecnologia de segurança de nuvem](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Prática recomendada de segurança do Azure 3-processo: atribuir responsabilidade por decisões de segurança na nuvem](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-5-define-network-security-strategy"></a>GS-5: definir a estratégia de segurança de rede

**Diretrizes**: estabeleça uma abordagem de segurança de rede do Azure como parte da estratégia geral de controle de acesso de segurança da sua organização.  

Essa estratégia deve incluir diretrizes documentadas, políticas e padrões para os seguintes elementos: 

-   Gerenciamento de rede centralizado e responsabilidade de segurança

-   Modelo de segmentação de rede virtual alinhado com a estratégia de segmentação corporativa

-   Estratégia de correção em diferentes cenários de ameaça e ataque

-   Borda da Internet e a estratégia de entrada e saída

-   Estratégia de interconectividade local e de nuvem híbrida

-   Artefatos de segurança de rede atualizados (por exemplo, diagramas de rede, arquitetura de rede de referência)

Para saber mais, consulte as referências a seguir:
- [Prática recomendada de segurança do Azure 11-arquitetura. Estratégia de segurança unificada única](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Benchmark de segurança do Azure-segurança de rede](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Visão geral da segurança de rede do Azure](../security/fundamentals/network-overview.md)

- [Estratégia de arquitetura de rede corporativa](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: definir a identidade e a estratégia de acesso privilegiado

**Orientação**: estabeleça uma identidade do Azure e abordagens de acesso privilegiado como parte da estratégia geral de controle de acesso de segurança da sua organização.  

Essa estratégia deve incluir diretrizes documentadas, políticas e padrões para os seguintes elementos: 

-   Um sistema de identidade e autenticação centralizado e sua interconectividade com outros sistemas de identidade internos e externos

-   Métodos de autenticação fortes em diferentes casos de uso e condições

-   Proteção de usuários altamente privilegiados

-   Monitoramento e manipulação de atividades de usuário de anomalias  

-   Identidade do usuário e processo de reavaliação e reconciliação de acesso

Para saber mais, consulte as referências a seguir:

- [Benchmark de segurança do Azure-gerenciamento de identidade](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Benchmark de segurança do Azure-acesso privilegiado](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Prática recomendada de segurança do Azure 11-arquitetura. Estratégia de segurança unificada única](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Visão geral da segurança de gerenciamento de identidade do Azure](../security/fundamentals/identity-management-overview.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: definir a estratégia de resposta contra ameaças e registro

**Orientação**: estabeleça uma estratégia de resposta a ameaças e registro para detectar e corrigir ameaças rapidamente, atendendo aos requisitos de conformidade. Priorize o fornecimento de analistas com alertas de alta qualidade e experiências integradas para que eles possam se concentrar em ameaças em vez de integração e etapas manuais. 

Essa estratégia deve incluir diretrizes documentadas, políticas e padrões para os seguintes elementos: 

-   A função e as responsabilidades da organização de operações de segurança (SecOps) 

-   Um processo de resposta a incidentes bem definido, alinhando-se ao NIST ou a outra estrutura do setor 

-   Captura de log e retenção para dar suporte à detecção de ameaças, resposta a incidentes e necessidades de conformidade

-   Visibilidade centralizada e informações de correlação sobre ameaças, usando SIEM, recursos nativos do Azure e outras fontes 

-   Plano de comunicação e notificação com seus clientes, fornecedores e partes públicas de seu interesse

-   Uso de plataformas nativas e de terceiros do Azure para tratamento de incidentes, como registro em log e detecção de ameaças, análise forense e remediação de ataque e erradicação

-   Processos para lidar com incidentes e atividades pós-incidente, como lições aprendidas e retenção de evidências

Para saber mais, consulte as referências a seguir:

- [Benchmark de segurança do Azure-registro em log e detecção de ameaças](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Benchmark de segurança do Azure-resposta de incidente](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Prática recomendada de segurança do Azure 4-Process. Atualizar processos de resposta a incidentes para a nuvem](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guia de decisão da estrutura de adoção, registro em log e relatórios do Azure](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Escala, gerenciamento e monitoramento do Azure Enterprise](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="next-steps"></a>Próximas etapas

- Consulte a [visão geral do benchmark de segurança do Azure v2](/azure/security/benchmarks/overview)
- Saiba mais sobre a [Linhas de base de segurança do Azure](/azure/security/benchmarks/security-baselines-overview)
