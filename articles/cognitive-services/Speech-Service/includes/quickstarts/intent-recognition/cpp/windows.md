---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: b4c6591911137441bec7411dc697ed63212fbbc2
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102445339"
---
## <a name="prerequisites"></a>Förutsättningar

Innan du börjar:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp" target="_blank">Installera tal-SDK för utvecklings miljön och skapa ett tomt exempel projekt</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Skapa en LUIS-app för avsikts igenkänning

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Öppna projektet i Visual Studio

Öppna sedan projektet i Visual Studio.

1. Starta Visual Studio 2019.
2. Läs in projektet och öppna `helloworld.cpp` .

## <a name="start-with-some-boilerplate-code"></a>Börja med viss exempel kod

Nu ska vi lägga till kod som fungerar som en Skeleton för vårt projekt. Observera att du har skapat en async-metod som kallas `recognizeIntent()` .

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,72-80)]

## <a name="create-a-speech-configuration"></a>Skapa en tal konfiguration

Innan du kan initiera ett `IntentRecognizer` objekt måste du skapa en konfiguration som använder nyckeln och platsen för din Luis förutsägelse resurs.

> [!IMPORTANT]
> Start nyckeln och redigerings nycklarna fungerar inte. Du måste använda din förutsägelse nyckel och plats som du skapade tidigare. Mer information finns i [skapa en Luis-app för avsikts igenkänning](#create-a-luis-app-for-intent-recognition).

Infoga den här koden i- `recognizeIntent()` metoden. Se till att du uppdaterar dessa värden:

* Ersätt `"YourLanguageUnderstandingSubscriptionKey"` med din Luis-förutsägelse nyckel.
* Ersätt `"YourLanguageUnderstandingServiceRegion"` med din Luis-plats.  Använd **regions identifierare** från [region](../../../../regions.md).

>[!TIP]
> Om du behöver hjälp med att hitta dessa värden kan du läsa [skapa en Luis-app för avsikts igenkänning](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

I det här exemplet används `FromSubscription()` metoden för att bygga `SpeechConfig` . En fullständig lista över tillgängliga metoder finns i [SpeechConfig-klass](/cpp/cognitive-services/speech/speechconfig).

Tal-SDK: n kommer att känna igen med en-US för språket, se [Ange käll språk för tal till text](../../../../how-to-specify-source-language.md) om du vill ha information om hur du väljer käll språk.

## <a name="initialize-an-intentrecognizer"></a>Initiera en IntentRecognizer

Nu ska vi skapa en `IntentRecognizer` . Infoga den här koden i `recognizeIntent()` metoden, precis under din tal konfiguration.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Lägg till ett LanguageUnderstandingModel och avsikter

Du måste associera en `LanguageUnderstandingModel` med avsikts igenkänningen och lägga till de avsikter som du vill identifiera. Vi ska använda avsikter från den färdiga domänen för start automatisering.

Infoga den här koden under din `IntentRecognizer` . Se till att du ersätter `"YourLanguageUnderstandingAppId"` med ditt Luis-app-ID.

>[!TIP]
> Om du behöver hjälp med att hitta det här värdet kan du läsa [skapa en Luis-app för avsikts igenkänning](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-33)]

I det här exemplet används `AddIntent()` funktionen för att individuellt lägga till avsikter. Använd och skicka modellen om du vill lägga till alla avsikter från en modell `AddAllIntents(model)` .

> [!NOTE]
> Tal-SDK stöder endast LUIS v 2.0-slut punkter.
> Du måste manuellt ändra slutpunkts-URL: en för v 3.0 som påträffades i exempel fråga fältet för att använda ett v 2.0 URL-mönster.
> LUIS v 2.0-slut punkter följer alltid ett av följande två mönster:
> * `https://{AzureResourceName}.cognitiveservices.azure.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
> * `https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`

## <a name="recognize-an-intent"></a>Identifiera en avsikt

Från `IntentRecognizer` -objektet kommer du att anropa- `RecognizeOnceAsync()` metoden. Med den här metoden kan röst tjänsten veta att du skickar en enda fras för igenkänning och att när frasen har identifierats för att sluta identifiera tal. För enkelhetens skull väntar vi på att framtiden ska bli klar.

Infoga den här koden under din modell:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=43)]

## <a name="display-the-recognition-results-or-errors"></a>Visa tolknings resultat (eller fel)

När igenkännings resultatet returneras av tal tjänsten vill du göra något med det. Vi ska hålla det enkelt och skriva ut resultatet till-konsolen.

Infoga den här koden nedan `auto result = recognizer->RecognizeOnceAsync().get();` :

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=46-71)]

## <a name="check-your-code"></a>Kontrol lera koden

Nu bör din kod se ut så här:

> [!NOTE]
> Vi har lagt till några kommentarer till den här versionen.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-79)]

## <a name="build-and-run-your-app"></a>Skapa och kör din app

Nu är du redo att bygga din app och testa vår tal igenkänning med röst tjänsten.

1. **Kompilera koden** – från meny raden i Visual Studio väljer du **bygge**  >  **build-lösning**.
2. **Starta din app** – från meny raden väljer du **Felsök**  >  **Starta fel sökning** eller tryck på <kbd>F5</kbd>.
3. **Starta igenkänning** – du uppmanas att tala en fras på engelska. Ditt tal skickas till tal tjänsten, skrivs som text och återges i-konsolen.

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [footer](./footer.md)]