---
title: funktioner i 4,0-Server version som stöds och syntax i Azure Cosmos DBs API för MongoDB
description: Lär dig mer om Azure Cosmos DBs API för MongoDB 4,0-Server version som stöds och syntax. Lär dig mer om databas kommandon, stöd för frågespråk, data typer, sammansättnings pipeline-kommandon och operatörer som stöds.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 03/02/2021
author: gahl-levy
ms.author: gahllevy
ms.openlocfilehash: 9eebc77c5b3d9402c766320fddfdaf05d50b574f
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102485409"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-40-server-version-supported-features-and-syntax"></a>Azure Cosmos DB s API för MongoDB (4,0-Server version): funktioner som stöds och syntax
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan kommunicera med Azure Cosmos DBens API för MongoDB med någon av MongoDB-klient [driv rutinerna](https://docs.mongodb.org/ecosystem/drivers)med öppen källkod. Azure Cosmos DB:s API för MongoDB tillåter gör det möjligt att använda befintliga klientdrivrutiner genom att följa MongoDB-[trådprotokollet](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Genom att använda Azure Cosmos DBs API för MongoDB kan du dra nytta av fördelarna med MongoDB som du använder för att, med alla Enterprise-funktioner som Cosmos DB ger: [global distribution](distribute-data-globally.md), [Automatisk horisontell partitionering](partitioning-overview.md), tillgänglighet och latens garantier, kryptering i vila, säkerhets kopiering och mycket mer.

## <a name="protocol-support"></a>Protokollstöd

De operatorer som stöds och eventuella begränsningar eller undantag anges nedan. Klientdrivrutiner som förstår dessa protokoll bör kunna ansluta till Cosmos DB:s API för MongoDB. När du använder Azure Cosmos DBs API för MongoDB-konton har de 3,6-versioner av kontona som har slut punkten i formatet, `*.mongo.cosmos.azure.com` medan 3,2-versionen av kontona har slut punkten i formatet `*.documents.azure.com` .

> [!NOTE]
> Den här artikeln listar de Server kommandon som stöds och omfattar inte funktioner på klient sidan. Klient sidans wrapper-funktioner som `deleteMany()` och `updateMany()` internt använder-och-Server- `delete()` `update()` kommandon. Funktioner som använder Server kommandon som stöds är kompatibla med Azure Cosmos DB s API för MongoDB.

## <a name="query-language-support"></a>Stöd för frågespråk

Azure Cosmos DB:s API för MongoDB erbjuder omfattande stöd för MongoDB-frågespråkskonstruktioner. Nedan hittar du en detaljerad lista över de åtgärder, operatorer, stadier, kommandon och alternativ som stöds för närvarande.

## <a name="database-commands"></a>Databaskommandon

Azure Cosmos DB:s API för MongoDB stöder följande databaskommandon:

### <a name="query-and-write-operation-commands"></a>Fråga och skriv-åtgärdskommandon

| Kommando | Stöds |
|---------|---------|
| [ändra strömmar](mongodb-change-streams.md) | Ja |
| delete | Ja |
| utvärdera | Inga |
| find | Ja |
| findAndModify | Ja |
| getLastError | Ja |
| getMore | Ja |
| getPrevError | Inga |
| insert | Ja |
| parallelCollectionScan | Inga |
| resetError | Inga |
| update | Ja |

### <a name="transaction-commands"></a>Transaktions kommandon

| Kommando | Stöds |
|---------|---------|
| abortTransaction | Ja |
| commitTransaction | Ja |

### <a name="authentication-commands"></a>Autentiseringskommandon

| Kommando | Stöds |
|---------|---------|
| authenticate | Ja |
| getnonce | Ja |
| logout | Ja |

### <a name="administration-commands"></a>Administrationskommandon

| Kommando | Stöds |
|---------|---------|
| cloneCollectionAsCapped | Inga |
| collMod | Inga |
| connectionStatus | Inga |
| convertToCapped | Inga |
| copydb | Inga |
| skapa | Ja |
| createIndexes | Ja |
| currentOp | Ja |
| drop | Ja |
| dropDatabase | Ja |
| dropIndexes | Ja |
| filemd5 | Ja |
| killCursors | Ja |
| killOp | Inga |
| listCollections | Ja |
| listDatabases | Ja |
| listIndexes | Ja |
| reIndex | Ja |
| renameCollection | Inga |

### <a name="diagnostics-commands"></a>Diagnostiska kommandon

| Kommando | Stöds |
|---------|---------|
| buildInfo | Ja |
| collStats | Ja |
| connPoolStats | Inga |
| connectionStatus | Inga |
| dataSize | Inga |
| dbHash | Inga |
| dbStats | Ja |
| Vad | Ja |
| funktioner | Inga |
| hostInfo | Ja |
| listDatabases | Ja |
| listCommands | Inga |
| profiler | Inga |
| serverStatus | Inga |
| top | Inga |
| whatsmyuri | Ja |

## <a name="aggregation-pipeline"></a><a name="aggregation-pipeline"></a>Sammansättnings pipeline

### <a name="aggregation-commands"></a>Sammansättningskommandon

| Kommando | Stöds |
|---------|---------|
| aggregate | Ja |
| count | Ja |
| distinct | Ja |
| mapReduce | Inga |

### <a name="aggregation-stages"></a>Sammansättningsfaser

| Kommando | Stöds |
|---------|---------|
| $addFields | Ja |
| $bucket | Inga |
| $bucketAuto | Inga |
| $changeStream | Ja |
| $collStats | Inga |
| $count | Ja |
| $currentOp | Inga |
| $facet | Ja |
| $geoNear | Ja |
| $graphLookup | Ja |
| $group | Ja |
| $indexStats | Inga |
| $limit | Ja |
| $listLocalSessions | Inga |
| $listSessions | Inga |
| $lookup | Ja |
| $match | Ja |
| $out | Ja |
| $project | Ja |
| $redact | Ja |
| $replaceRoot | Ja |
| $replaceWith | Inga |
| $sample | Ja |
| $skip | Ja |
| $sort | Ja |
| $sortByCount | Ja |
| $unwind | Ja |

### <a name="boolean-expressions"></a>Booleska uttryck

| Kommando | Stöds |
|---------|---------|
| $and | Ja |
| $not | Ja |
| $or | Ja |

### <a name="conversion-expressions"></a>Konverterings uttryck

| Kommando | Stöds |
|---------|---------|
| $convert | Ja |
| $toBool | Ja |
| $toDate | Ja |
| $toDecimal | Ja |
| $toDouble | Ja |
| $toInt | Ja |
| $toLong | Ja |
| $toObjectId | Ja |
| $toString | Ja |

### <a name="set-expressions"></a>Uttryck för angivelse

| Kommando | Stöds |
|---------|---------|
| $setEquals | Ja |
| $setIntersection | Ja |
| $setUnion | Ja |
| $setDifference | Ja |
| $setIsSubset | Ja |
| $anyElementTrue | Ja |
| $allElementsTrue | Ja |

### <a name="comparison-expressions"></a>Jämförelseuttryck

| Kommando | Stöds |
|---------|---------|
| $cmp | Ja |
| $eq | Ja | 
| $gt | Ja | 
| $gte | Ja | 
| $lt | Ja |
| $lte | Ja | 
| $ne | Ja | 
| $in | Ja | 
| $nin | Ja | 

### <a name="arithmetic-expressions"></a>Aritmetiska uttryck

| Kommando | Stöds |
|---------|---------|
| $abs | Ja |
| $add | Ja |
| $ceil | Ja |
| $divide | Ja |
| $exp | Ja |
| $floor | Ja |
| $ln | Ja |
| $log | Ja |
| $log10 | Ja |
| $mod | Ja |
| $multiply | Ja |
| $pow | Ja |
| $sqrt | Ja |
| $subtract | Ja |
| $trunc | Ja |

### <a name="string-expressions"></a>Stränguttryck

| Kommando | Stöds |
|---------|---------|
| $concat | Ja |
| $indexOfBytes | Ja |
| $indexOfCP | Ja |
| $ltrim | Ja |
| $rtrim | Ja |
| $trim | Ja |
| $split | Ja |
| $strLenBytes | Ja |
| $strLenCP | Ja |
| $strcasecmp | Ja |
| $substr | Ja |
| $substrBytes | Ja |
| $substrCP | Ja |
| $toLower | Ja |
| $toUpper | Ja |

### <a name="text-search-operator"></a>Text Sök operator

| Kommando | Stöds |
|---------|---------|
| $meta | Inga |

### <a name="array-expressions"></a>Matrisuttryck

| Kommando | Stöds |
|---------|---------|
| $arrayElemAt | Ja |
| $arrayToObject | Ja |
| $concatArrays | Ja |
| $filter | Ja |
| $indexOfArray | Ja |
| $isArray | Ja |
| $objectToArray | Ja |
| $range | Ja |
| $reverseArray | Ja |
| $reduce | Ja |
| $size | Ja |
| $slice | Ja |
| $zip | Ja |
| $in | Ja |

### <a name="variable-operators"></a>Variabel operatörer

| Kommando | Stöds |
|---------|---------|
| $map | Ja |
| $let | Ja |

### <a name="system-variables"></a>Systemvariabler

| Kommando | Stöds |
|---------|---------|
| $ $CURRENT | Ja |
| $ $DESCEND | Ja |
| $ $KEEP | Ja |
| $ $PRUNE | Ja |
| $ $REMOVE | Ja |
| $ $ROOT | Ja |

### <a name="literal-operator"></a>Litteral operator

| Kommando | Stöds |
|---------|---------|
| $literal | Ja |

### <a name="date-expressions"></a>Datumuttryck

| Kommando | Stöds |
|---------|---------|
| $dayOfYear | Ja |
| $dayOfMonth | Ja |
| $dayOfWeek | Ja |
| $year | Ja |
| $month | Ja | 
| $week | Ja |
| $hour | Ja |
| $minute | Ja | 
| $second | Ja |
| $millisecond | Ja | 
| $dateToString | Ja |
| $isoDayOfWeek | Ja |
| $isoWeek | Ja |
| $dateFromParts | Inga | 
| $dateToParts | Inga |
| $dateFromString | Inga |
| $isoWeekYear | Ja |

### <a name="conditional-expressions"></a>Villkorliga uttryck

| Kommando | Stöds |
|---------|---------|
| $cond | Ja |
| $ifNull | Ja |
| $switch | Ja |

### <a name="data-type-operator"></a>Data typs operator

| Kommando | Stöds |
|---------|---------|
| $type | Ja |

### <a name="accumulator-expressions"></a>Ackumulator uttryck

| Kommando | Stöds |
|---------|---------|
| $sum | Ja |
| $avg | Ja |
| $first | Ja |
| $last | Ja |
| $max | Ja |
| $min | Ja |
| $push | Ja |
| $addToSet | Ja |
| $stdDevPop | Ja |
| $stdDevSamp | Ja |

### <a name="merge-operator"></a>Slå samman operatör

| Kommando | Stöds |
|---------|---------|
| $mergeObjects | Ja |

## <a name="data-types"></a>Datatyper

Azure Cosmos DBs API för MongoDB stöder dokument som är kodade i MongoDB BSON-format. 4,0 API-versionen förbättrar den interna användningen av det här formatet för att förbättra prestanda och minska kostnaderna. Dokument som skrivits eller uppdateras via en slut punkt som kör 4,0-förmåner från detta.
 
I ett [uppgraderings scenario](mongodb-version-upgrade.md)kommer dokument som skrivits före uppgraderingen till version 4,0 inte att dra nytta av den förbättrade prestandan förrän de har uppdaterats via en Skriv åtgärd genom 4,0-slutpunkten.

| Kommando | Stöds |
|---------|---------|
| Double | Ja |
| Sträng | Ja |
| Objekt | Ja |
| Matris | Ja |
| Binära data | Ja | 
| ObjectId | Ja |
| Boolesk | Ja |
| Datum | Ja |
| Null | Ja |
| 32-bitars heltal (int) | Ja |
| Timestamp | Ja |
| 64-bitars heltal (långt) | Ja |
| MinKey | Ja |
| MaxKey | Ja |
| Decimal128 | Ja | 
| Reguljärt uttryck | Ja |
| JavaScript | Ja |
| Java Script (med omfång)| Ja |
| Undefined (Odefinierad) | Ja |

## <a name="indexes-and-index-properties"></a>Index och index egenskaper

### <a name="indexes"></a>Index

| Kommando | Stöds |
|---------|---------|
| Index för enskilt fält | Ja |
| Sammansatt index | Ja |
| MultiKey-index | Ja |
| Text index | Inga |
| 2dsphere | Ja |
| 2D-index | Inga |
| Hashed index | Ja |

### <a name="index-properties"></a>Index egenskaper

| Kommando | Stöds |
|---------|---------|
| TTL | Ja |
| Unik | Ja |
| Delvis | Inga |
| Skift läges okänslig | Inga |
| Utspridda | Inga |
| Bakgrund | Ja |

## <a name="operators"></a>Operatorer

### <a name="logical-operators"></a>Logiska operatorer

| Kommando | Stöds |
|---------|---------|
| $or | Ja |
| $and | Ja |
| $not | Ja |
| $nor | Ja | 

### <a name="element-operators"></a>Element operatörer

| Kommando | Stöds |
|---------|---------|
| $exists | Ja |
| $type | Ja |

### <a name="evaluation-query-operators"></a>Operatorer för utvärderings fråga

| Kommando | Stöds |
|---------|---------|
| $expr | Inga |
| $jsonSchema | Inga |
| $mod | Ja |
| $regex | Ja |
| $text | Nej (stöds inte. Använd $regex i stället.)| 
| $where | Inga | 

I $regex-frågor tillåter vänster-förankrade uttryck indexs ökning. Om du använder modifierarna i (skiftlägesokänsligt) och m (flera rader) så får du dock samlingsskanningen i alla uttryck.

När det finns ett behov att inkludera $ eller | så är det bäst att skapa två (eller flera) regex-frågor. Till exempel, med följande ursprungliga fråga: `find({x:{$regex: /^abc$/})` , måste ändras på följande sätt:

`find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})`

Den första delen använder indexet för att begränsa sökningen till dokument som börjar med ^abc och den andra delen matchar de exakta posterna. Fält-operatorn | fungerar som en or-funktion – frågan `find({x:{$regex: /^abc |^def/})` matchar dokumenten där fältet x har värden som börjar med abc eller def. Om du vill använda indexet så rekommenderar vi att du delar upp frågan i två olika frågor anslutna med operatorn $or: `find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })`.

### <a name="array-operators"></a>Mat ris operatorer

| Kommando | Stöds | 
|---------|---------|
| $all | Ja | 
| $elemMatch | Ja | 
| $size | Ja | 

### <a name="comment-operator"></a>Kommentars operator

| Kommando | Stöds | 
|---------|---------|
| $comment | Ja | 

### <a name="projection-operators"></a>Projektions operatörer

| Kommando | Stöds |
|---------|---------|
| $elemMatch | Ja |
| $meta | Inga |
| $slice | Ja |

### <a name="update-operators"></a>Uppdateringsoperatorer

#### <a name="field-update-operators"></a>Fältuppdateringsoperatorer

| Kommando | Stöds |
|---------|---------|
| $inc | Ja |
| $mul | Ja |
| $rename | Ja |
| $setOnInsert | Ja |
| $set | Ja |
| $unset | Ja |
| $min | Ja |
| $max | Ja |
| $currentDate | Ja |

#### <a name="array-update-operators"></a>Matrisuppdateringsoperatorer

| Kommando | Stöds |
|---------|---------|
| $ | Ja |
| $[]| Ja |
| $[<identifier>]| Ja |
| $addToSet | Ja |
| $pop | Ja |
| $pullAll | Ja |
| $pull | Ja |
| $push | Ja |
| $pushAll | Ja |

#### <a name="update-modifiers"></a>Uppdatera modifierare

| Kommando | Stöds |
|---------|---------|
| $each | Ja |
| $slice | Ja |
| $sort | Ja |
| $position | Ja |

#### <a name="bitwise-update-operator"></a>Binär uppdateringsoperator

| Kommando | Stöds |
|---------|---------|
| $bit | Ja | 
| $bitsAllSet | Inga |
| $bitsAnySet | Inga |
| $bitsAllClear | Inga |
| $bitsAnyClear | Inga |

### <a name="geospatial-operators"></a>Geospatiala operatorer

Operator | Stöds | 
--- | --- |
$geoWithin | Ja |
$geoIntersects | Ja | 
$near | Ja |
$nearSphere | Ja |
$geometry | Ja |
$minDistance | Ja |
$maxDistance | Ja |
$center | Inga |
$centerSphere | Inga |
$box | Inga |
$polygon | Inga |

## <a name="sort-operations"></a>Sorterings åtgärder

Med åtgärden `findOneAndUpdate` kan du använda sorteringsåtgärder på ett enda fält, men inte på flera fält.

## <a name="unique-indexes"></a>Unika index

[Unika index](mongodb-indexing.md#unique-indexes) säkerställer att ett särskilt fält inte har dubblettvärden i alla dokument i en samling, på liknande sätt som unika bevaras i standard nyckeln "_ID". Du kan skapa unika index i Azure Cosmos DB genom att använda `createIndex` kommandot med `unique` begränsnings parametern:

```javascript
globaldb:PRIMARY> db.coll.createIndex( { "amount" : 1 }, {unique:true} )
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 4
}
```

## <a name="compound-indexes"></a>Sammansatta index

[Sammansatta index](mongodb-indexing.md#compound-indexes-mongodb-server-version-36) ger ett sätt att skapa ett index för fält grupper i upp till åtta fält. Den här typen av index skiljer sig från de interna MongoDB-sammansatta indexen. I Azure Cosmos DB används sammansatta index för sorterings åtgärder som tillämpas på flera fält. Om du vill skapa ett sammansatt index måste du ange mer än en egenskap som parameter:

```javascript
globaldb:PRIMARY> db.coll.createIndex({"amount": 1, "other":1})
{
    "createdCollectionAutomatically" : false, 
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 2,
    "ok" : 1
}
```

## <a name="gridfs"></a>GridFS

Azure Cosmos DB stöder GridFS via en GridFS-kompatibel Mongo-drivrutin.

## <a name="replication"></a>Replikering

Azure Cosmos DB stöder automatisk, inbyggd replikering på de understa lagren. Denna logik utökas för att även uppnå global replikering med låga svarstider. Cosmos DB stöder inte manuella replikeringskommandon.

## <a name="retryable-writes"></a>Försök till skrivningar

Cosmos DB stöder ännu inte försök att skriva på nytt. Klient driv rutiner måste lägga till URL-parametern "retryWrites = false" i sin anslutnings sträng. Du kan lägga till fler URL-parametrar genom att prefixet med &. 

## <a name="sharding"></a>Horisontell partitionering

Azure Cosmos DB stöder automatisk, horisontell partitionering på serversidan. Den hanterar Shard skapande, placering och balansering automatiskt. Azure Cosmos DB stöder inte manuella horisontell partitionering-kommandon, vilket innebär att du inte behöver anropa kommandon som addShard, balancerStart, moveChunk osv. Du behöver bara ange nyckeln Shard när du skapar behållarna eller frågar efter data.

## <a name="sessions"></a>Sessioner

Azure Cosmos DB har ännu inte stöd för kommandon på Server sidans sessioner.

## <a name="time-to-live-ttl"></a>TTL-värde (time to live)

Azure Cosmos DB stöder ett TTL-värde (Time-to-Live) baserat på dokumentets tidsstämpel. Du kan aktivera TTL för samlingar genom att gå till [Azure Portal](https://portal.azure.com).

## <a name="transactions"></a>Transaktioner

Transaktioner med flera dokument stöds i en unsharded-samling. Transaktioner med flera dokument stöds inte i samlingar eller i shardade-samlingar. Tids gränsen för transaktioner är en fast 5 sekunder.

## <a name="user-and-role-management"></a>Användar- och rollhantering

Azure Cosmos DB stöder ännu inte användare och roller. Cosmos DB stöder dock rollbaserad åtkomst kontroll i Azure (Azure RBAC) och skriv-och skriv skydds-och skrivskyddade lösen ord/nycklar som kan hämtas via [Azure Portal](https://portal.azure.com) (anslutnings sträng).

## <a name="write-concern"></a>Skrivanmärkningar

Vissa program är beroende av [Skriv](https://docs.mongodb.com/manual/reference/write-concern/)åtgärder, vilket anger antalet svar som krävs vid en Skriv åtgärd. På grund av hur Cosmos DB hanterar replikering i bakgrunden är alla skrivningar automatiskt kvorumskrivningar som standard. Skrivanmärkningar som anges av klientkoden ignoreras. Läs mer om hur du [maximerar tillgänglighet och prestanda med hjälp av konsekvensnivåer](consistency-levels.md).

## <a name="next-steps"></a>Nästa steg

- Lär dig hur du [använder Studio 3T](mongodb-mongochef.md) med Azure Cosmos DB:s API för MongoDB.
- Lär dig hur du [använder Robo 3T](mongodb-robomongo.md) med Azure Cosmos DB:s API för MongoDB.
- Utforska MongoDB-[exempel](mongodb-samples.md) med Azure Cosmos DB:s API för MongoDB.
