---
ms.openlocfilehash: 1e3ba4b39baa045f35c232fa97c14bc78d8de5ca
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88691111"
---
1. I Visual Studio Code går du till *src/Edge*. Du ser *. kuvert* -filen och några mallar för distributions mal len.

    Distributions mal len refererar till distributions manifestet för gräns enheten, där variabler används för vissa egenskaper. *. Kuvert* -filen innehåller värdena för variablerna.
1. Gå till mappen *src/Cloud-to-Device-console-app* . Här ser du *appsettings.jspå* filen och några andra filer:

    * ***C2D-console-app. CSPROJ*** – projekt filen för Visual Studio Code.
    * ***operations.js*** en lista över de åtgärder som du vill att programmet ska köra.
    * ***Program.cs*** – exempel koden. Den här koden:
    
      * Läser in appinställningar.
      * Anropar direkta metoder som exponeras av direktsänd video analys i IoT Edge modul. Du kan använda modulen för att analysera direktuppspelade video strömmar genom att anropa dess [direkta metoder](../../../direct-methods.md).
      * Pausar så att du kan granska programmets utdata i **terminalfönstret** och granska de händelser som genererats av modulen i fönstret **utdata** .
      * Anropar direkta metoder för att rensa resurser.   
