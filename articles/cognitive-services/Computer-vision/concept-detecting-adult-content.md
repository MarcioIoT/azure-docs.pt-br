---
title: Adulto, erótico, conteúdo de terríveis Pesquisa Visual Computacional
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados à detecção de conteúdo adulto em imagens usando a APi Pesquisa Visual Computacional.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5cc8a4508ceeda245fbc10a81e16f3ecf05284c7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95013602"
---
# <a name="detect-adult-content"></a>Detectar conteúdo adulto

Pesquisa Visual Computacional pode detectar material adulto em imagens para que os desenvolvedores possam restringir a exibição dessas imagens em seu software. Os sinalizadores de conteúdo são aplicados com uma pontuação entre zero e um para que os desenvolvedores possam interpretar os resultados de acordo com suas próprias preferências.

> [!NOTE]
> Grande parte dessa funcionalidade é oferecida pelo serviço de [Content moderator do Azure](../content-moderator/overview.md) . Confira essa alternativa para soluções para cenários de moderação de conteúdo mais rigorosos, como a moderação de texto e fluxos de trabalho de análise humana.

## <a name="content-flag-definitions"></a>Definições de sinalizador de conteúdo

Dentro da classificação "adulto" há várias categorias diferentes:

- As imagens **adultas** são definidas como aquelas que são explicitamente sexuais por natureza e muitas vezes descrevem nudez e atos sexuais.
- As imagens **erótico** são definidas como imagens que são obscenas com natureza e geralmente contêm conteúdo menos sexualmente explícito que as imagens marcadas como **adultas**.
- As imagens **terríveis** são definidas como aquelas que descrevem lo.

## <a name="use-the-api"></a>Usar a API

Você pode detectar conteúdo adulto com a API de [análise de imagem](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) . Quando você adiciona o valor de `Adult` ao parâmetro de consulta **visualFeatures** , a API retorna três propriedades booleanas &mdash; `isAdultContent` , `isRacyContent` e `isGoryContent` &mdash; em sua resposta JSON. O método também retorna propriedades correspondentes &mdash; `adultScore` , `racyScore` e `goreScore` &mdash; que representam pontuações de confiança entre zero e um para cada categoria respectiva.

- [Início rápido: analisar uma imagem (SDK do .NET)](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Início rápido: analisar uma imagem (API REST)](./quickstarts/csharp-analyze.md)