---
title: Hämta en signerings nyckel från en princip .NET
description: Det här avsnittet visar hur du hämtar en signerings nyckel från den befintliga principen med hjälp av Media Services v3 .NET SDK.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 1436561f7c82446038c231fadec3bd62c94d4ff9
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98955113"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Hämta en signeringsnyckel från den befintliga principen

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En av de viktigaste designprinciperna för v3 API är att göra API:et säkrare. v3-API: er returnerar inte hemligheter eller autentiseringsuppgifter för **Get** -eller **list** åtgärder. Se den detaljerade förklaringen här: Mer information finns i [Azure RBAC-och Media Services-konton](rbac-overview.md)

Exemplet i den här artikeln visar hur du använder .NET för att hämta en signerings nyckel från den befintliga principen. 
 
## <a name="download"></a>Ladda ned 

Klona en GitHub-lagringsplats som innehåller det fullständiga .NET-exemplet på din dator med hjälp av följande kommando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Exemplet på ContentKeyPolicy med hemligheter finns i mappen [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) .

## <a name="get-contentkeypolicy-with-secrets"></a>Hämta ContentKeyPolicy med hemligheter 

Använd **GetPolicyPropertiesWithSecretsAsync**, som du ser i exemplet nedan, för att komma till nyckeln.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Nästa steg

[Utforma multi-DRM-innehållsskyddssystem med åtkomstkontroll](design-multi-drm-system-with-access-control.md) 
