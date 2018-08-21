---
title: Valideren van de oorspronkelijke leveranciers (OEM)-pakketten in de Azure Stack-validatie als een service | Microsoft Docs
description: Informatie over het controleren van de oorspronkelijke leveranciers (OEM)-pakketten met validatie uit als een service.
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
ms.openlocfilehash: a08f439780e0080d8da2cde1531e1580dbdad14f
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235370"
---
# <a name="validate-oem-packages"></a>OEM-pakketten valideren

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Wanneer er een wijziging in de firmware of stuurprogramma's voor de validatie van een voltooide oplossing is, kunt u een nieuw pakket voor OEM testen. Wanneer de test van het pakket is verstreken, wordt het ondertekend door Microsoft. Uw test moet het bijgewerkte OEM-uitbreidingspakket met de stuurprogramma's en firmware die zijn geslaagd voor Windows Server-logo en pc's tests bevatten.

Alle tests zijn voltooid binnen 24 uur met het resultaat van **geslaagd**. Als een van de tests een resultaat van zijn **mislukt**, een bug in [Microsoft Collaborate](https://aka.ms/collaborate) en Microsoft laat weten door te sturen een e-mail naar [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

## <a name="get-your-oem-package-signed"></a>Uw OEM-pakket is ondertekend

1. Zorg ervoor dat de huidige maandelijkse update is toegepast. Zie voor de nieuwste versie, de meest recente versie in [documentatie over Azure Stack-Operator > overzicht > Opmerkingen bij de Release](https://docs.microsoft.com/azure/azure-stack/) .

    Software-updates van Microsoft met Azure Stack worden aangeduid met behulp van een naamconventie geldt, bijvoorbeeld voor maart 2018 1803 die wijzen op de update is. Voor meer informatie over het beleid voor Azure Stack update uitgebracht en release-opmerkingen beschikbaar zijn, Zie [Azure Stack servicebeleid](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

1. System health-status controleren door te voeren **Test AzureStack** zoals beschreven in het vak [uitvoeren een validatietest voor Azure Stack. Alle waarschuwingen en fouten oplossen voordat u een test start.

2. In de [validatie portal](https://azurestackvalidation.com), selecteer een bestaande oplossing. Als u uw oplossing niet hebt toegevoegd, Zie [toevoegen van een nieuwe oplossing](azure-stack-vaas-validate-solution-new.md#add-a-new-solution).

3. Selecteer **Start** op de **pakket validaties** tegel om een nieuwe werkstroom te starten.

    ![Pakket validaties](media/image3.png)

4.  Diagnostische gegevens van een verbindingsreeks opgeven. Zie voor instructies [instellen van een storage-account](azure-stack-vaas-set-up-account.md).

    Een pakket met OEM-uitbreiding moet worden opgegeven voor elke validatie van het pakket worden uitgevoerd. Geef het OEM-pakket dat is geïnstalleerd op de oplossing op het moment van Azure Stack-implementatie. Zie voor instructies [maken van een Azure storage-blob voor het opslaan van logboeken](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    Een JSON-bestand met de omgevingsvariabelen die moet worden gebruikt voor het voltooien van de invoer voor de vereiste velden voor het uitvoeren om te voorkomen dat fouten in de invoer van gegevens. Zie voor instructies [ophalen van een configuratiebestand in de implementatie van een Azure Stack](azure-stack-vaas-parameters.md).

5. Voer de tests uit.

6. Wanneer alle tests zijn voltooid, stuurt u de naam van uw oplossing en pakket validatie [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) om aan te vragen pakket ondertekenen.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
