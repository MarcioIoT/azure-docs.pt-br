---
title: Criar uma conta de armazenamento para Azure Data Lake Storage Gen2
description: Saiba como criar uma conta de armazenamento para uso com Azure Data Lake Storage Gen2.
author: normesta
ms.topic: how-to
ms.author: normesta
ms.date: 08/31/2020
ms.service: storage
ms.reviewer: stewu
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 6b63933fc625079bb490942cf3a32232a484fe38
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89269995"
---
# <a name="create-a-storage-account-to-use-with-azure-data-lake-storage-gen2"></a>Criar uma conta de armazenamento para usar com Azure Data Lake Storage Gen2

Para usar Data Lake Storage Gen2 recursos, crie uma conta de armazenamento que tenha um namespace hierárquico.

## <a name="choose-a-storage-account-type"></a>Escolher um tipo de conta de armazenamento

Data Lake Storage recursos têm suporte nos seguintes tipos de contas de armazenamento:

- Uso geral v2
- BlockBlobStorage

Para obter informações sobre como escolher entre elas, consulte [visão geral da conta de armazenamento](../common/storage-account-overview.md).

## <a name="create-a-storage-account-with-a-hierarchical-namespace"></a>Criar uma conta de armazenamento com um namespace hierárquico

Crie uma [conta de uso geral v2](../common/storage-account-create.md) ou uma conta [BlockBlobStorage](storage-blob-create-account-block-blob.md) com a configuração de **namespace hierárquico** habilitada.

Desbloqueie Data Lake Storage recursos ao criar a conta habilitando a configuração de **namespace hierárquico** na guia **avançado** da página **criar conta de armazenamento** . Você deve habilitar essa configuração ao criar a conta. Você não pode habilitá-lo posteriormente.

A imagem a seguir mostra essa configuração na página **criar conta de armazenamento** .

> [!div class="mx-imgBorder"]
> ![Configuração de namespace hierárquico](./media/create-data-lake-storage-account/hierarchical-namespace-feature.png)

Se você tiver uma conta de armazenamento existente que deseja usar com Data Lake Storage e a configuração de namespace hierárquico estiver desabilitada, você deverá migrar os dados para uma nova conta de armazenamento que tenha a configuração habilitada.

## <a name="next-steps"></a>Próximas etapas

- [Visão geral da conta de armazenamento](../common/storage-account-overview.md)
- [Usando o Azure Data Lake Storage Gen2 para exigências de big data](data-lake-storage-data-scenarios.md)
- [Controle de acesso no Azure Data Lake Storage Gen2](data-lake-storage-access-control.md)