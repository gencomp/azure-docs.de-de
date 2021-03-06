---
title: 'Verhindern der Übersetzung von Inhalten: Translator'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit Translator die Übersetzung von Inhalten verhindern. Mit Translator können Sie Inhalte kennzeichnen, die nicht übersetzt werden sollen.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 37d2e4c0131569ab50ebf49ff73b6adf7a420713
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996175"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>Verhindern der Übersetzung von Inhalten mit Translator

Mit Translator können Sie Inhalte kennzeichnen, die nicht übersetzt werden sollen. Sie können beispielsweise Code, einen Markennamen oder einzelne Wörter/Ausdrücke markieren, deren Übersetzung nicht sinnvoll ist.

## <a name="methods-for-preventing-translation"></a>Methoden zum Verhindern der Übersetzung

1. Kennzeichnen Sie Ihren Inhalt mit `notranslate`. Dies funktioniert prinzipbedingt nur, wenn der Eingabetexttyp auf HTML festgelegt ist.

   Beispiel:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. Verwenden Sie das [dynamische Wörterbuch](dynamic-dictionary.md), um eine bestimmte Übersetzung vorzuschreiben.

3. Übergeben Sie die Zeichenfolge nicht zur Übersetzung an Translator.

4. Benutzerdefinierter Translator: Verwenden Sie ein [Wörterbuch in „Benutzerdefinierter Translator“](custom-translator/what-is-dictionary.md), um die Übersetzung eines Ausdrucks mit einer Wahrscheinlichkeit von 100% vorzuschreiben.


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Vermeiden einer Übersetzung in Ihrem Translator-Aufruf](reference/v3-0-translate.md)
