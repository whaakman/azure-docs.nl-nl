---
title: Een test met Azure Stack-validatie als een service bewaken | Microsoft Docs
description: Een test met Azure Stack-validatie als een service bewaken.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 2dc4d3f2855864ff80648b5b9635ff28c0dacbb7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163323"
---
# <a name="monitor-a-test-with-azure-stack-validation-as-a-service"></a>Een test met Azure Stack-validatie als een service bewaken

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

De uitvoering van een test kan worden bewaakt via het **Operations** -pagina voor test-pakketten die worden uitgevoerd of voltooid. Deze pagina informatie over de status van de test en de bewerkingen.

## <a name="monitor-a-test"></a>Een test controleren

1. Selecteer een oplossing.

2. Selecteer **beheren** op een willekeurige tegel werkstroom.

3. Klik op een werkstroom om de samenvatting test-pagina te openen.

4. Vouw het contextmenu **[...]**  testen voor elk exemplaar van de suite.

5. Selecteer **bewerkingen weergeven**

![Alternatieve tekst](media\image4.png)

Voor de tests die zijn voltooid, logboeken kunnen worden gedownload op de pagina overzicht van testen door te klikken op **logboeken downloaden die u** in het contextmenu van de test **[...]** . Azure Stack-partners kunnen deze Logboeken gebruiken om op te sporen problemen met mislukte tests.

## <a name="open-the-test-pass-summary"></a>Open de pass testsamenvatting

1. Open de portal. 
2. Selecteer de naam van een bestaande oplossing met eerder uitvoeren of geplande tests.

    ![Test passen beheren](media/managetestpasses.png)

3. Selecteer **beheren** in de **Test passen** deelvenster.
4. Selecteer de testronde de pass testsamenvatting openen. U kunt de testnaam van de bekijken datum gemaakt, worden uitgevoerd, hoe lang de test heeft ondernomen, en het resultaat (geslaagd of mislukt).
5. Selecteer [ **...  .** ].

### <a name="test-pass-summary"></a>Pass-testsamenvatting

| Kolom | Beschrijving |
| --- | --- |
| Testnaam | De naam van de test. Verwijst naar het nummer van de validatie. |
| Gemaakt | Tijd waarop die de test-pas is gemaakt. |
| Gestart | Wanneer die de laatste test is uitgevoerd. |
| Duur | Lengte van de tijd die het heeft de testronde om uit te voeren. |
| Status | Het resultaat (geslaagd of mislukt) voor de rest-pas. |
| Naam van agent | De volledig gekwalificeerde domeinnaam van de agent. |
| Totaal aantal bewerkingen | Het totale aantal bewerkingen uitgevoerd in de testronde. |
| Doorgegeven bewerkingen | Het aantal bewerkingen die in de testronde doorgegeven. |
|  Mislukte bewerkingen | Het aantal bewerkingen dat is mislukt. |

### <a name="group-columns-in-the-test-pass-summary"></a>Kolommen in de test groep doorgeven samenvatting

U kunt selecteren en sleept u een kolom in de koptekst van een groep maken op de waarde in de kolom.

## <a name="reschedule-a-test"></a>Een test plannen

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **plannen** te plannen van de testronde.
3. Voer het beheerderswachtwoord van de Cloud voor uw Azure Stack-exemplaar.
4. Voer de diagnostische gegevens van Storage-verbindingsreeks die u hebt gedefinieerd bij het instellen van uw account.
5. Plannen van de test.

## <a name="cancel-a-test"></a>Een test annuleren

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **annuleren**.

## <a name="get-test-information"></a>Testinformatie ophalen

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **informatie weergeven** te plannen van de testronde.

**Testgegevens**

| Naam | Beschrijving |
| -- | -- |
| Testnaam | De naam van de test, bijvoorbeeld OEM-Update op Azure Stack 1806 RC validatie. |
| Testversie | De versie van de test, bijvoorbeeld 5.1.4.0. |
| Uitgever | De test-uitgever, zoals Microsoft. |
| Categorie | De categorie van de test, zoals **functionele** of **betrouwbaarheid**. |
| Doelservices | De services die zijn getest, zoals virtuele machines |
| Beschrijving | De beschrijving van de test. |
| Geschatte duur (minuten) | De lengte van de tijd in minuten de test duurde om uit te voeren. |
| Koppelingen | Een koppeling naar GitHub Issue Tracker waar. |

## <a name="get-test-parameters"></a>Testparameters ophalen

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **parameters weergeven** te plannen van de testronde.

**Parameters**

| Naam | Beschrijving |
| -- | -- |
| Testnaam | De naam van de test, bijvoorbeeld oemupdate1806test. |
| Testversie | De versie van de rest, bijvoorbeeld 5.1.4.0. |
| Test exemplaar-ID | Een GUID waarmee het specifieke exemplaar van de test, bijvoorbeeld 20b20645-b400-4f0d-bf6f-1264d866ada9. |
| cloudAdminUser | De naam van het account dat wordt gebruikt als beheerder van de cloud, bijvoorbeeld **cloudadmin**. |
| DiagnosticsContainerName | De ID van de diagnostische container, bijvoorbeeld 04dd3815-5f35-4158-92ea-698027693080. |

## <a name="get-test-operations"></a>Testbewerkingen ophalen

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **bewerkingen weergeven** te plannen van de testronde. Het deelvenster van de operations-overzicht wordt geopend.

## <a name="get-test-logs"></a>Test-logboeken ophalen

1. [Open de pass testsamenvatting](#open-the-test-pass-summary).
2. Selecteer **logboeken downloaden die u** te plannen van de testronde.  
    Een zip-bestand met de naam ReleaseYYYY-MM-DD.zip met het downloaden van de logboeken.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
