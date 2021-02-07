---
title: Identifiera och utforska data i ADF med avdelningens kontroll
description: Lär dig hur du identifierar, utforskar data i Azure Data Factory med avdelningens kontroll
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: lrtoyou1223
ms.author: lle
manager: shwang
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.openlocfilehash: accb9bbf195daa3d25e1aed109e36ef309083385
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/07/2021
ms.locfileid: "99805317"
---
# <a name="discover-and-explore-data-in-adf-using-purview"></a>Identifiera och utforska data i ADF med avdelningens kontroll

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

I den här artikeln registrerar du ett Azure avdelningens kontroll-konto till ett Data Factory. Med den anslutningen kan du identifiera Azure avdelningens kontroll-tillgångar och interagera med dem via ADF-funktioner. 

Du kan utföra följande uppgifter i ADF: 
- Använd sökrutan längst upp för att hitta avdelningens kontroll-tillgångar baserat på nyckelord 
- Förstå data baserat på metadata, härkomst, anteckningar 
- Anslut dessa data till din data fabrik med länkade tjänster eller data uppsättningar 

## <a name="prerequisites"></a>Förutsättningar 
- [Azure avdelningens kontroll-konto](../purview/create-catalog-portal.md) 
- [Data Factory](./quickstart-create-data-factory-portal.md) 
- [Anslut ett Azure avdelningens kontroll-konto till Data Factory](./connect-data-factory-to-azure-purview.md) 

## <a name="using-azure-purview-in-data-factory"></a>Använda Azure avdelningens kontroll i Data Factory 

Använd Azure-avdelningens kontroll i Data Factory kräver att du har åtkomst till det avdelningens kontroll-kontot. Data Factory passerar avdelningens kontroll-behörigheten. Om du till exempel har en curator-behörighet kan du redigera metadata som genomsöks av Azure avdelningens kontroll. 

### <a name="data-discovery-search-datasets"></a>Data identifiering: Sök data uppsättningar 

Om du vill identifiera data som registrerats och genomsökts av Azure avdelningens kontroll kan du använda Sök fältet längst upp i mitten av Data Factory Portal. Se till att du väljer Azure-avdelningens kontroll för att söka efter alla organisations data. 

:::image type="content" source="./media/data-factory-purview/search-dataset.png" alt-text="Skärm bild för att utföra över data uppsättningar.":::

### <a name="actions-that-you-can-perform-over-datasets-with-data-factory-resources"></a>Åtgärder som du kan utföra över data uppsättningar med Data Factory resurser 
Du kan direkt skapa länkad tjänst, data uppsättning eller data flöde med de data du söker via Azure avdelningens kontroll.

:::image type="content" source="./media/data-factory-purview/actions-over-purview-data.png" alt-text="Skärm bild som visar hur du kan skapa länkad tjänst, data uppsättning eller data flöde direkt via de data du söker via Azure avdelningens kontroll.":::

##  <a name="nextsteps"></a>Nästa steg 

- [Registrera och skanna Azure Data Factory till gångar i Azure avdelningens kontroll](../purview/register-scan-azure-synapse-analytics.md)
- [Söka efter data i Azure avdelningens kontroll Data Catalog](../purview/how-to-search-catalog.md)