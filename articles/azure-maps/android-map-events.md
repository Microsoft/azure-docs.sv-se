---
title: Hantera kart händelser i Android Maps | Microsoft Azure Maps
description: Lär dig vilka händelser som utlöses när användarna interagerar med Maps. Visa en lista över alla mappnings händelser som stöds. Se hur du använder Azure Maps Android SDK för att hantera händelser.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 86d1b9ec8a507a5cfaa5502efcb239bceabca665
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097354"
---
# <a name="interact-with-the-map-android-sdk"></a>Interagera med kartan (Android SDK)

Den här artikeln visar hur du använder Maps Events Manager.

## <a name="interact-with-the-map"></a>Interagera med kartan

Kartan hanterar alla händelser via dess `events` egenskap. I följande tabell visas alla mappnings händelser som stöds.

| Händelse                  | Händelse hanterarens format | Beskrivning |
|------------------------|----------------------|-------------|
| `OnCameraIdle`         | `()`                 | <p>Utlöses efter att den sista bild rutan renderades innan kartan övergår i tillståndet "inaktive":<ul><li>Ingen kamera över gång pågår.</li><li>Alla begärda paneler har lästs in.</li><li>Alla animeringar av tona/över gångar har slutförts.</li></ul></p> |
| `OnCameraMove`         | `()`                 | Utlöses upprepade gånger under en animerad över gång från en vy till en annan, som ett resultat av en användar interaktion eller metoder. |
| `OnCameraMoveCanceled` | `()`                 | Utlöses när en flyttnings förfrågan till kameran har avbrutits. |
| `OnCameraMoveStarted`  | `(int reason)`       | Utlöses precis innan kartan påbörjar en över gång från en vy till en annan, som ett resultat av en användar interaktion eller metoder. `reason`Argumentet för händelse lyssnaren returnerar ett heltals värde som innehåller information om hur kamera rörelsen initierades. I följande lista beskrivs möjliga orsaker:<ul><li>1: gest</li><li>2: Developer-animering</li><li>3: API-animering</li></ul>   |
| `OnClick`              | `(double lat, double lon)` | Utlöses när kartan trycks ned och släpps på samma plats på kartan. |
| `OnFeatureClick`       | `(List<Feature>)`    | Utlöses när kartan trycks ned och släpps på samma ställe i en funktion.  |
| `OnLayerAdded` | `(Layer layer)` | Utlöses när ett lager läggs till i kartan. |
| `OnLayerRemoved` | `(Layer layer)` | Utlöses när ett lager tas bort från kartan. |
| `OnLoaded` | `()` | Utlöses omedelbart efter att alla nödvändiga resurser har hämtats och den första visuella åter givningen av kartan har inträffat. |
| `OnLongClick`          | `(double lat, double lon)` | Utlöses när kartan trycks ned, hålls i ett ögonblick och släpps sedan på samma plats på kartan. |
| `OnLongFeatureClick `  | `(List<Feature>)`    | Utlöses när kartan trycks ned, hålls i ett ögonblick och släpps sedan på samma ställe i en funktion. |
| `OnReady`              | `(AzureMap map)`     | Utlöses när kartan ursprungligen läses in eller när appens orientering ändras och de lägsta obligatoriska kart resurserna läses in och kartan är klar att program mässigt interagera med. |
| `OnSourceAdded` | `(Source source)` | Utlöses när en `DataSource` eller `VectorTileSource` läggs till i kartan. |
| `OnSourceRemoved` | `(Source source)` | Utlöses när en `DataSource` eller tas `VectorTileSource` bort från kartan. |
| `OnStyleChange` | `()` | Utlöses när kartans format läses in eller ändras. |

Följande kod visar hur du lägger till `OnClick` -, `OnFeatureClick` -och- `OnCameraMove` händelser till kartan.

::: zone pivot="programming-language-java-android"

```java
map.events.add((OnClick) (lat, lon) -> {
    //Map clicked.
});

map.events.add((OnFeatureClick) (features) -> {
    //Feature clicked.
});

map.events.add((OnCameraMove) () -> {
    //Map camera moved.
});
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
map.events.add(OnClick { lat: Double, lon: Double -> 
    //Map clicked.
})

map.events.add(OnFeatureClick { features: List<Feature?>? -> 
    //Feature clicked.
})

map.events.add(OnCameraMove {
    //Map camera moved.
})
```

::: zone-end

Mer information finns i Översikt över [kart](how-to-use-android-map-control-library.md#navigating-the-map) dokumentationen om hur du interagerar med kartan och utlöser händelser.

## <a name="scope-feature-events-to-layer"></a>Omfattnings funktions händelser till lager

När du lägger till `OnFeatureClick` eller `OnLongFeatureClick` -händelserna i kartan kan en lager instans eller ett lager-ID skickas in som en andra parameter. När ett lager skickas i, utlöses händelsen bara om händelsen inträffar på det lagret. Händelser som är begränsade till lager stöds av symbol-, bubbeldiagram-, linje-och polygonens lager.

::: zone pivot="programming-language-java-android"

```java
//Create a data source.
DataSource source = new DataSource();
map.sources.add(source);

//Add data to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a layer and add it to the map.
BubbleLayer layer = new BubbleLayer(source);
map.layers.add(layer);

//Add a feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnFeatureClick) (features) -> {
    //One or more features clicked.
}, layer);

//Add a long feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnLongFeatureClick) (features) -> {
    //One or more features long clicked.
}, layer);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source.
val source = DataSource()
map.sources.add(source)

//Add data to the data source.
source.add(Point.fromLngLat(0, 0))

//Create a layer and add it to the map.
val layer = BubbleLayer(source)
map.layers.add(layer)

//Add a feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add(
    OnFeatureClick { features: List<Feature?>? -> 
        //One or more features clicked.
    },
    layer
)

//Add a long feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add(
    OnLongFeatureClick { features: List<Feature?>? -> 
         //One or more features long clicked.
    },
    layer
)
```

::: zone-end

## <a name="next-steps"></a>Nästa steg

I följande artiklar finns fullständiga kod exempel:

> [!div class="nextstepaction"]
> [Navigera i kartan](how-to-use-android-map-control-library.md#navigating-the-map)

> [!div class="nextstepaction"]
> [Lägga till ett symbolskikt](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Lägga till ett bubbelskikt](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Lägg till ett linjeskikt](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Lägg till ett polygonskikt](how-to-add-shapes-to-android-map.md)
