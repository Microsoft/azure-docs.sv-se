---
title: Logga ML-experiment och mått
titleSuffix: Azure Machine Learning
description: Aktivera loggning på din ML-utbildning kör för att övervaka körnings mått i real tid och för att diagnostisera fel och varningar.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 9e5f64d9ef61a272da488ad70e690db4c07ddccc
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/06/2021
ms.locfileid: "99625085"
---
# <a name="enable-logging-in-ml-training-runs"></a>Aktivera loggning i ML utbildning körs


Med Python-SDK:n i Azure Machine Learning kan du logga information i realtid med hjälp av både standardpaketet för Python-loggning och SDK-specifika funktioner. Du kan logga lokalt och skicka loggar till din arbetsyta i portalen.

Med loggarna kan du diagnostisera fel och varningar, eller spåra prestandamått som parametrar och modellprestanda. I den här artikeln får du lära dig hur du aktiverar loggning i följande scenarier:

> [!div class="checklist"]
> * Interaktiva träningssessioner
> * Skicka träningsjobb med ScriptRunConfig
> * Inbyggda `logging`-inställningar i Python
> * Loggning från fler källor


> [!TIP]
> Den här artikeln visar hur du övervakar modellträningsprocessen. Om du vill veta mer om att övervaka resursanvändning och händelser från Azure Machine Learning, till exempel kvoter, slutförda träningskörningar eller slutförda modelldistributioner, kan du läsa [Övervakning i Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="data-types"></a>Datatyper

Du kan logga flera datatyper, inklusive skalära värden, listor, tabeller, bilder, kataloger med mera. Mer information och Python-kodexempel för olika datatyper finns på [referenssidan Körningsklass](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py).

## <a name="interactive-logging-session"></a>Interaktiv loggningssession

Interaktiva loggningssessioner används vanligtvis i miljöer med notebook-filer. Metoden [Experiment.start_logging()](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truestart-logging--args----kwargs-) startar en interaktiv loggningssession. Alla mått som loggas under sessionen läggs till i körningsposten i experimentet. Metoden [run.complete()](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truecomplete--set-status-true-) avslutar sessionerna och markerar körningen som slutförd.

## <a name="scriptrun-logs"></a>ScriptRun-loggar

I det här avsnittet lär du dig att lägga till loggningskod i körningar som skapas vid konfiguration med ScriptRunConfig. Du kan använda [**ScriptRunConfig**](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py)-klassen till att kapsla in skript och miljöer för upprepningsbara körningar. Du kan också använda det här alternativet till att visa widgeten för Jupyter Notebooks vid övervakning.

I det här exemplet utförs en parameterrensning av alfavärden och resultaten samlas in med hjälp av metoden [run.log()](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truelog-name--value--description----).

1. Skapa ett träningsskript som innehåller loggningslogiken `train.py`.

   [!code-python[](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. Skicka ```train.py```-skriptet för körning i en användarhanterad miljö. Hela skriptmappen skickas för träningen.

   [! Notebook – python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? Name = src)]


   [! Notebook-python [] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb? namn = kör)]

    Parametern `show_output` aktiverar utförlig loggning så att du kan se information från träningsprocessen, samt information om eventuella fjärresurser eller beräkningsmål. Använd följande kod för att aktivera utförlig loggning när du skickar experimentet.

```python
run = exp.submit(src, show_output=True)
```

Du kan också använda samma parameter i funktionen `wait_for_completion` för den resulterande körningen.

```python
run.wait_for_completion(show_output=True)
```

## <a name="native-python-logging"></a>Intern Python-loggning

Vissa loggar i SDK:n kan innehålla ett fel som uppmanar dig att ange loggningsnivån DEBUG. Om du ska ange loggningsnivån lägger du till nedanstående kod i skriptet.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="additional-logging-sources"></a>Fler loggningskällor

Azure Machine Learning kan också logga information från andra källor under träningen, till exempel automatiserade maskininlärningskörningar eller Docker-containrar som kör jobben. Dessa loggar dokumenteras inte, men om du stöter på problem och kontaktar Microsofts support kan de använda loggarna vid felsökning.

Information om loggning av mått i Azure Machine Learning-designern finns i [Loggning av mått i designern](how-to-track-designer-experiments.md)

## <a name="example-notebooks"></a>Exempelnotebook-filer

Följande notebook-filer demonstrerar begreppen i den här artikeln:
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nästa steg

Läs de här artiklarna om du vill lära dig mer om att använda Azure Machine Learning:

* Lär dig att [logga mått i Azure Machine Learning-designern](how-to-track-designer-experiments.md).

* Se ett exempel på hur du registrerar den bästa modellen och distribuerar den i självstudien [Träna en bildklassificeringsmodell med Azure Machine Learning](tutorial-train-models-with-aml.md).
