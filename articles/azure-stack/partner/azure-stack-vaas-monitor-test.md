---
title: Controleren en beheren van tests in de portal voor Azure Stack VaaS | Microsoft Docs
description: Controleren en beheren van tests in de portal voor Azure Stack VaaS.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 04756481b676ed198120b67ca4368093ca8acead
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250871"
---
# <a name="monitor-and-manage-tests-in-the-vaas-portal"></a>Controleren en beheren van tests in de portal VaaS

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Na het plannen van tests op basis van uw Azure Stack-oplossing, zullen validatie uit als een Service (VaaS) reporting uitvoeringsstatus van de test. Deze informatie is beschikbaar in de portal VaaS, samen met de acties, zoals opnieuw plannen en tests wordt geannuleerd.

## <a name="navigate-to-the-workflow-tests-summary-page"></a>Navigeer naar de pagina voor een overzicht van de werkstroom testen

1. Selecteer een bestaande oplossing met ten minste één werkstroom op het dashboard oplossingen.

    ![Werkstroom-tegels](media/tile_all-workflows.png)

1. Selecteer **beheren** op de tegel van de werkstroom. De volgende pagina geeft een lijst van de werkstromen die zijn gemaakt voor de geselecteerde oplossing.

1. Selecteer de naam van de werkstroom te openen van de samenvatting test.

## <a name="change-workflow-parameters"></a>Werkstroomparameters wijzigen

Elk werkstroomtype kunt u bewerken de [Webtestparameters](azure-stack-vaas-parameters.md#test-parameters) opgegeven tijdens het maken van de werkstroom.

1. Selecteer op de overzichtspagina van tests, de **bewerken** knop.

1. Geef nieuwe waarden volgens [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md).

1. Selecteer **indienen** om op te slaan van de waarden.

> [!NOTE]
> In de **testronde** werkstroom, moet u de selectie van de test voltooid en gaat u naar de pagina controleren voordat u de nieuwe parameterwaarden kunt opslaan.

### <a name="add-tests-test-pass-only"></a>Tests (alleen testronde) toevoegen

In **testronde** werkstromen, zowel de **tests toevoegen** en **bewerken** knoppen kunt u nieuwe tests in de werkstroom plannen.

> [!TIP]
> Selecteer **tests toevoegen** als u alleen nieuwe tests plannen en hoeft niet te bewerken van parameters voor een **testronde** werkstroom.

## <a name="managing-test-instances"></a>Test-instanties beheren

Voor niet-officiële worden uitgevoerd (dat wil zeggen, de **testronde** werkstroom), de overzichtspagina voor de tests geeft een lijst van de tests gepland op basis van de Azure Stack-oplossing.

Voor de officiële wordt uitgevoerd (dat wil zeggen, de **validatie** werkstromen), de overzichtspagina voor de tests geeft een lijst van de tests die zijn vereist voor het uitvoeren van validatie van de Azure Stack-oplossing. Validatietests zijn gepland op deze pagina.

Elk exemplaar van de geplande test bevat de volgende informatie:

| Kolom | Beschrijving |
| --- | --- |
| Testnaam | De naam en versie van de test. |
| Categorie | Het doel van de test. |
| Gemaakt | De tijd waarbinnen de test is gepland. |
| Gestart | De tijd waarbinnen de test heeft de uitvoering. |
| Duur | De hoeveelheid tijd die de test is uitgevoerd. |
| Status | De status of het resultaat van de test. Pre execution of in uitvoering statussen zijn: `Pending`, `Running`. Terminal statussen zijn: `Cancelled`, `Failed`, `Aborted`, `Succeeded`. |
| Naam van agent | De naam van de agent die de test is uitgevoerd. |
| Totaal aantal bewerkingen | Het totale aantal bewerkingen uitgevoerd tijdens de test. |
| Doorgegeven bewerkingen | Het aantal bewerkingen die tijdens de test is geslaagd. |
|  Mislukte bewerkingen | Het aantal bewerkingen die tijdens de test is mislukt. |

### <a name="actions"></a>Acties

Het exemplaar van elke test een lijst met beschikbare acties die u uitvoeren kunt wanneer u klikt op in het contextmenu **[...]**  in de tabel test-exemplaren.

#### <a name="view-information-about-the-test-definition"></a>Informatie over de test-definitie weergeven

Selecteer **informatie weergeven** in het contextmenu om algemene informatie over de definitie van de test weer te geven. Dit wordt gedeeld door elk exemplaar van de test met dezelfde naam en versie.

| De eigenschap testen | Beschrijving |
| -- | -- |
| Testnaam | De naam van de test. |
| Testversie | De versie van de test. |
| Uitgever | De uitgever van de test. |
| Categorie |  Het doel van de test. |
| Doelservices | De Azure Stack-services worden getest. |
| Beschrijving | De beschrijving van de test. |
| Geschatte duur (minuten) | De verwachte runtime van de test. |
| Koppelingen | Eventuele relevante informatie over de test of de punten van de contactpersoon. |

#### <a name="view-test-instance-parameters"></a>Parameters voor test-exemplaar weergeven

Selecteer **parameters weergeven** in het contextmenu om de opgegeven met het exemplaar van de test tijdens de planning parameters weer te geven. Gevoelige tekenreeksen zoals wachtwoorden worden niet weergegeven. Deze actie is alleen beschikbaar voor tests die zijn gepland.

In dit venster bevat de volgende metagegevens voor alle exemplaren van de test:

| Instantie-eigenschap testen | Beschrijving |
| -- | -- |
| Testnaam | De naam van de test. |
| Testversie | De versie van de test. |
| Test exemplaar-ID | Een GUID voor het identificeren van het specifieke exemplaar van de test. |

#### <a name="view-test-instance-operations"></a>Bewerkingen voor test-exemplaar weergeven

Selecteer **bewerkingen weergeven** uit de context door menu om een gedetailleerde status van bewerkingen weer te geven tijdens de test wordt uitgevoerd. Deze actie is alleen beschikbaar voor tests die zijn gepland.

![bewerkingen weergeven](media/manage-test_context-menu-operations.png)

#### <a name="download-logs-for-a-completed-test-instance"></a>Logboeken voor een test is voltooid-exemplaar downloaden

Selecteer **logboeken downloaden die u** in het contextmenu voor het downloaden van een `.zip` -bestand van de logboeken uitvoer tijdens het uitvoeren van de test. Deze actie is alleen beschikbaar voor tests die hebt voltooid, dat wil zeggen, een test met de status van een van beide `Cancelled`, `Failed`, `Aborted`, of `Succeeded`.

#### <a name="reschedule-a-test-instance-or-schedule-a-test"></a>Plannen van een test-exemplaar of een test plannen

Planning van tests op de beheerpagina, is afhankelijk van het type van de werkstroom dat de test wordt uitgevoerd onder.

##### <a name="test-pass-workflow"></a>Testronde werkstroom

In de werkstroom testronde **vluchtstatus** een test-exemplaar wordt gebruikgemaakt van dezelfde set parameters als de oorspronkelijke test-instantie en *vervangt* het oorspronkelijke resultaat, met inbegrip van de logboeken. U moet gevoelige tekenreeksen zoals wachtwoorden opnieuw in wanneer u opnieuw plannen.

1. Selecteer **plannen** in het contextmenu om een prompt voor het plannen van de test-exemplaar te openen.

1. Geef de toepasselijke parameters.

1. Selecteer **indienen** plannen van de test-instantie en vervang de bestaande instantie.

##### <a name="validation-workflows"></a>Validatie van werkstromen

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

#### <a name="cancel-a-test-instance"></a>Annuleren van een test-exemplaar

Een geplande test worden geannuleerd als de status ervan `Pending` of `Running`.  

1. Selecteer **annuleren** in het contextmenu om een prompt voor het annuleren van de test-exemplaar te openen.

1. Selecteer **indienen** annuleren van de test-exemplaar.

## <a name="next-steps"></a>Volgende stappen

- [Validatie as a Service oplossen](azure-stack-vaas-troubleshoot.md)
