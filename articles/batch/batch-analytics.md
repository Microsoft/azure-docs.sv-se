---
title: Azure Batch analys
description: Ämnena i batch Analytics innehåller referensinformation om de händelser och aviseringar som är tillgängliga för batch-tjänstens resurser.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: 0d55ecd7f10e267a9cb469dffcdf26c131c8ce41
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91849519"
---
# <a name="batch-analytics"></a>Batchanalys

Ämnena i det här avsnittet innehåller referensinformation om de händelser och aviseringar som är tillgängliga för batch-tjänstens resurser.

Mer information om hur du aktiverar och använder batch-diagnostikloggar finns i [Azure Batch diagnostisk loggning](./batch-diagnostics.md) .

## <a name="diagnostic-logs"></a>Diagnostikloggar

Azure Batchs tjänsten genererar följande diagnostiska logg händelser under livstiden för vissa batch-resurser.

### <a name="service-log-events"></a>Tjänst logg händelser

- [Skapa pool](batch-pool-create-event.md)
- [Start för borttagning av pool](batch-pool-delete-start-event.md)
- [Borttagning av pool slutförd](batch-pool-delete-complete-event.md)
- [Start av poolens storleks ändring](batch-pool-resize-start-event.md)
- [Storleks ändring av pool slutförd](batch-pool-resize-complete-event.md)
- [Autoskalning av pool](batch-pool-autoscale-event.md)
- [Uppgiftens start](batch-task-start-event.md)
- [Uppgiften slutförd](batch-task-complete-event.md)
- [Åtgärden kunde inte utföras](batch-task-fail-event.md)
- [Aktivitets schema fungerar inte](batch-task-schedule-fail-event.md)
