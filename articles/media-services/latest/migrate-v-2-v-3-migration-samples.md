---
title: Jämförelse av exempel på Media Services v2 till v3-migrering
description: En uppsättning exempel som hjälper dig att jämföra kod skillnaderna mellan Azure Media Services v2 och v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2021
ms.author: inhenkel
ms.openlocfilehash: 640b9b40295ae9b9aea865f7b6159da6ff4a3251
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898315"
---
# <a name="media-services-migration-code-sample-comparison"></a>Exempel jämförelse för Media Services migrations kod

![logo typ för migrations guide](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

Du kan använda några av våra kod exempel för att jämföra hur saker görs mellan SDK: er.

## <a name="samples-for-comparison"></a>Exempel för jämförelse

Följande tabell är en lista över exempel för jämförelse mellan v2 och v3 för vanliga scenarier.

|Scenario|v2-API|v3-API|
|---|---|---|
|Skapa en till gång och ladda upp en fil |[v2 .NET-exempel](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[v3 .NET-exempel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Skicka ett jobb|[v2 .NET-exempel](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[v3 .NET-exempel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Visar hur du först skapar en transformering och sedan skickar ett jobb.|
|Publicera en till gång med AES-kryptering |1. skapa `ContentKeyAuthorizationPolicyOption`<br/>2. skapa `ContentKeyAuthorizationPolicy`<br/>3. skapa `AssetDeliveryPolicy`<br/>4. skapa `Asset` och ladda upp innehåll eller skicka `Job` och Använd `OutputAsset`<br/>5. associera `AssetDeliveryPolicy` med `Asset`<br/>6. skapa `ContentKey`<br/>7. Koppla `ContentKey` till `Asset`<br/>8. skapa `AccessPolicy`<br/>9. skapa `Locator`<br/><br/>[v2 .NET-exempel](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. skapa `ContentKeyPolicy`<br/>2. skapa `Asset`<br/>3. Ladda upp innehåll eller Använd `Asset` som `JobOutput`<br/>4. skapa `StreamingLocator`<br/><br/>[v3 .NET-exempel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|Hämta jobb information och hantera jobb |[Hantera jobb med v2](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[Hantera jobb med v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
