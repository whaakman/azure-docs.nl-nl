---
title: De opmaak van documenten en naamgevingsregels - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Dit is een handleiding voor de opmaak van documenten en naamgeving in aangepaste Translator. Dit concept helpt bij het beheren van documenten namen betere abd naamgevingsconflicten voorkomen.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 74b421691f5d3536c44af16e99dc54451dfde3f7
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872761"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Documenteer indelingen en naming convention-richtlijnen

Elk bestand dat wordt gebruikt voor aangepaste vertaling moet ten minste **vier** tekens bestaan.

Deze tabel bevat alle ondersteunde bestandsindelingen die u gebruiken kunt voor het bouwen van uw systeem vertaling:

| Indeling            | Extensies   | Beschrijving                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | . XLF. XLIFF | Een parallelle documentindeling exporteren van het geheugen van de vertaling systemen. De talen die worden gebruikt zijn in het bestand gedefinieerd.                                                                                                                                                              |
| TMX               | . TMX         | Een parallelle documentindeling exporteren van het geheugen van de vertaling systemen. De talen die worden gebruikt zijn in het bestand gedefinieerd.                                                                                                                                                              |
| POSTCODE               | . POSTCODE         | ZIP wordt een archiefbestand.                                                                                                                                                                                                        |
| Locstudio         | . LCL         | Een Microsoft-indeling voor parallelle documenten                                                                                                                                                                                                                                      |
| Microsoft Word    | . DOCX        | Microsoft Word-document                                                                                                                                                                                                                                                        |
| Adobe Acrobat     | . PDF-BESTAND         | Adobe Acrobat portable document                                                                                                                                                                                                                                                |
| HTML              | . HTML-CODE. HTM  | HTML-document                                                                                                                                                                                                                                                                  |
| Tekstbestand         | . TXT         | UTF-16- of UTF-8-gecodeerde tekstbestanden                                                                                                                                                                                                                                             |
| Uitgelijnde tekstbestand | . UITLIJNEN       | De extensie `.ALIGN` is een speciale uitbreiding die u gebruiken kunt als u weet dat de zinnen in het document paar perfect zijn uitgelijnd. Als u een `.ALIGN` zinnen wordt niet in overeenstemming gebracht aangepaste Translator-bestand voor u. |
| Excel-bestand        | . XLSX        | Excel-bestand (2013 of later). Eerste regel / rij van het werkblad de taalcode die moet worden.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Woordenlijst indelingen

Woordenboeken ondersteuning aangepaste Translator voor alle bestandsindelingen die worden ondersteund voor de trainingset. Als u Excel-woordenboek gebruikt, zorg ervoor dat de eerste regel / rij van het werkblad moet taalcodes.

## <a name="zip-file-formats"></a>ZIP-bestandsindelingen

Documenten kunnen worden gegroepeerd in één zip-bestand en geüpload. De aangepaste Translator ondersteunt zip-indelingen (ZIP GZ en TGZ).

Elk document in het zip-bestand met de extensie, TXT, HTML, HTM, PDF, DOCX, uitlijnen moet volgen deze naamconventie:

{documentnaam} \_{taalcode} wanneer {document name} de naam van uw document is, {taalcode} is de ISO-LanguageID (twee tekens), die aangeeft dat het document zinnen in die taal bevat. Er moet een onderstrepingsteken (_) voordat de taalcode.

Bijvoorbeeld, als u wilt uploaden twee parallelle documenten in een zip voor een Engels, Spaans systeem, de bestanden moeten de naam 'data_en' en 'data_es'.

Vertaalbestanden van geheugen (TMX, XLF, XLIFF, LCL, XLSX) hoeven niet te volgen de naamconventie voor een specifieke taal.  

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [project](workspace-and-project.md#what-is-a-custom-translator-project) maken en deze beheren.
