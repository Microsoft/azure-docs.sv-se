---
title: Visa hälso status för Azure NetApp Files replikeringsrelation | Microsoft Docs
description: Beskriver hur du visar replikeringsstatus på käll volymen eller mål volymen för Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: a14656bc1abc42fdc3e948fdae1ca07469681d21
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/22/2020
ms.locfileid: "95249860"
---
# <a name="display-health-status-of-replication-relationship"></a>Visa hälsostatus för replikeringsrelation 

Du kan visa replikeringsstatus på käll volymen eller mål volymen.  

## <a name="display-replication-status"></a>Visa replikeringsstatus

1. Klicka på **replikering** under lagrings tjänst för volym på antingen käll volymen eller mål volymen.

    Följande replikeringsstatus och hälso information visas:  
    * **Slut punkts typ** – anger om volymen är källan eller målet för replikeringen.
    * **Health** – visar en hälso status för replikeringsrelationen.
    * **Speglings tillstånd** – visar ett av följande värden:
        * Ej *initierad*:  
            Detta är det första och standard läget när en peering-relation skapas. Statusen är inte initierad förrän initieringen har slutförts.
        * *Speglad*:   
            Mål volymen har initierats och det är dags att ta emot speglings uppdateringar.
        * *Bruten*:   
            Det här är läget när du har delat upp peering-relationen. Mål volymen är `‘RW’` och ögonblicks bilder finns.
    * **Relations status** – visar ett av följande värden: 
        * *Inaktiv*:  
            Ingen överförings åtgärd pågår och framtida överföringar har inte inaktiverats.
        * *Överföring*:  
            En överförings åtgärd pågår och framtida överföringar har inte inaktiverats.
    * **Replikeringsschema** – visar hur ofta stegvisa stegvisa speglings uppdateringar ska utföras när initieringen (bas linje kopian) har slutförts.

    * **Totalt förlopp** – visar den totala mängden data i byte som överförts för den aktuella överförings åtgärden. Den här mängden är den faktiska BITS som överförs och kan skilja sig från det logiska utrymmet som käll-och mål volymerna rapporterar.  

    ![Hälso status för replikering](../media/azure-netapp-files/cross-region-replication-health-status.png)

> [!NOTE] 
> Replikeringsrelationen *visar hälso status som ohälsosam om tidigare* replikeringar inte har slutförts. Den här statusen är ett resultat av stora volymer som överförs med ett mindre överförings fönster (till exempel en överförings tid på tio minuter för en stor volym). I det här fallet visar Relations status *överföring* och hälso status är i fel *tillstånd*.

## <a name="next-steps"></a>Nästa steg  

* [Replikering mellan regioner](cross-region-replication-introduction.md)
* [Hantera haveriberedskap](cross-region-replication-manage-disaster-recovery.md)
* [Mått för volym replikering](azure-netapp-files-metrics.md#replication)
* [Ta bort volymer eller volymer](cross-region-replication-delete.md)
* [Felsöka replikering mellan regioner](troubleshoot-cross-region-replication.md)

