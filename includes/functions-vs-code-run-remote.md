---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2021
ms.author: glenga
ms.openlocfilehash: 121b10cc568cce089c90e66b9c6f292c74f4acbe
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809577"
---
## <a name="run-the-function-in-azure"></a>Kör funktionen i Azure

1. Gå tillbaka till avsnittet **Azure: Functions** i sido fältet, expandera din prenumeration, din nya Function-app och **functions.** Högerklicka (Windows) eller <kbd>CTRL-</kbd> klicka (MacOS) på `HttpExample` funktionen och välj **Kör funktion nu..**..

    :::image type="content" source="media/functions-vs-code-run-remote/execute-function-now.png" alt-text="Kör funktionen nu i Azure från Visual Studio Code":::

1. I **Ange brödtext för begäran** visas bröd texten för begär ande meddelandet `{ "name": "Azure" }` . Skicka meddelandet till din funktion genom att trycka på RETUR.  

1. När funktionen körs i Azure och returnerar ett svar, aktive ras ett meddelande i Visual Studio Code.
