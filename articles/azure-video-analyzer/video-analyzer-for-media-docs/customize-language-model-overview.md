---
title: Anpassen eines Sprachmodells in Azure Video Analyzer for Media (früher Video Indexer) – Azure
titleSuffix: Azure Video Analyzer for Media
description: Dieser Artikel vermittelt einen Überblick darüber, was ein Sprachmodell in Azure Video Analyzer for Media (früher Video Indexer) ist und wie es angepasst werden kann.
services: azure-video-analyzer
author: anikaz
manager: johndeu
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 05/15/2019
ms.author: kumud
ms.openlocfilehash: ca79d7d541d137a299fd61431df9410c6822be04
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112121376"
---
# <a name="customize-a-language-model-with-video-analyzer-for-media"></a>Anpassen eines Sprachmodells mit Video Analyzer for Media

Azure Video Analyzer for Media (früher Video Indexer) unterstützt die automatische Spracherkennung durch die Integration in Microsoft [Custom Speech Service](https://azure.microsoft.com/services/cognitive-services/custom-speech-service/). Sie können das Sprachmodell anpassen, indem Sie den Anpassungstext hochladen. Dies ist Text aus der Domäne, an dessen Vokabular sich das Modul anpassen soll. Nachdem Sie Ihr Modell trainiert haben, werden neue Wörter aus dem Anpassungstext erkannt, und die Standardaussprache wird übernommen. Darüber hinaus lernt das Sprachmodell neue Wortsequenzen, für die die Wahrscheinlichkeit des Auftretens hoch ist. Benutzerdefinierte Sprachmodelle werden für Englisch, Spanisch, Französisch, Deutsch, Italienisch, Chinesisch (vereinfacht), Japanisch, Russisch, Portugiesisch, Hindi und Koreanisch unterstützt. 

Wir verwenden ein sehr spezifisches Wort wie „Kubernetes“ (im Kontext des Azure Kubernetes Service) als Beispiel. Da das Wort für Video Analyzer for Media neu ist, wird es als „Communitys“ erkannt. Sie müssen das Modell trainieren, damit „Kubernetes“ richtig erkannt wird. In anderen Fällen sind die Wörter zwar vorhanden, aber das Sprachmodell erwartet ihr Vorkommen in einem bestimmten Kontext nicht. Im Englischen handelt es sich bei „container service“ beispielsweise nicht um eine Folge von zwei Wörtern, die von einem nicht speziell trainierten Sprachmodell als spezifische Wortfolge erkannt wird.

Sie haben die Möglichkeit, Wörter ohne Kontext in einer Liste als Textdatei hochzuladen. Dies wird als Teilanpassung angesehen. Alternativ hierzu können Sie auch Textdateien mit einer Dokumentation oder mit Sätzen hochladen, die zu Ihrem Inhalt passen, um eine bessere Anpassung zu erzielen.

Sie können die Video Analyzer for Media-APIs oder die Website verwenden, um benutzerdefinierte Sprachmodelle zu erstellen und zu bearbeiten. Dies ist in den Themen beschrieben, die in diesem Artikel unter [Nächste Schritte](#next-steps) angegeben sind.

## <a name="best-practices-for-custom-language-models"></a>Bewährte Methoden für benutzerdefinierte Sprachmodelle

Video Analyzer for Media lernt basierend auf der Wahrscheinlichkeit von Wortkombinationen. Beachten Sie also Folgendes, um den besten Lernerfolg zu erzielen:

* Geben Sie eine ausreichende Zahl von Beispielsätzen an, wie sie in der Realität ausgesprochen werden.
* Geben Sie pro Zeile nur einen Satz an. Andernfalls erlernt das System satzübergreifende Wahrscheinlichkeiten.
* Es auch in Ordnung, nur ein Wort als Satz anzugeben, um dem Wort gegenüber anderen Wörtern mehr Gewicht zu verleihen. Das System lernt aber am besten anhand von vollständigen Sätzen.
* Geben Sie beim Hinzufügen neuer Wörter oder Akronyme nach Möglichkeit ebenso viele Beispiele für die Nutzung in einem vollständigen Satz an, damit das System über möglichst viel Kontext verfügt.
* Probieren Sie mehrere Anpassungsoptionen aus, um zu ermitteln, welche für Sie gut funktionieren.
* Vermeiden Sie es, identische Sätze mehrfach anzugeben. Dies kann dazu führen, dass diese Sätze gegenüber den restlichen Eingaben ein höheres Gewicht erhalten.
* Vermeiden Sie es, ungewöhnliche Symbole (~, # @ % &) anzugeben, da sie verworfen werden. Auch die Sätze, in denen die Symbole vorkommen, werden verworfen.
* Vermeiden Sie zu umfangreiche Eingaben, z.B. Hunderte oder Tausende von Sätzen, weil sonst der Verstärkungseffekt abgeschwächt wird.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Sprachmodells mit APIs](customize-language-model-with-api.md)

[Anpassen des Sprachmodells mit der Website](customize-language-model-with-website.md)
