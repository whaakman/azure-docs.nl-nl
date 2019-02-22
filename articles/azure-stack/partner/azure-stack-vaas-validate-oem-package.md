---
title: Valideren van de oorspronkelijke leveranciers (OEM)-pakketten in de Azure Stack-validatie als een Service | Microsoft Docs
description: Informatie over het valideren van de oorspronkelijke leveranciers (OEM)-pakketten met validatie uit als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 02/19/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f9ed10c84be86304722020606873b0c7866df1e8
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594046"
---
# <a name="validate-oem-packages"></a>OEM-pakketten valideren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Wanneer er een wijziging in de firmware of stuurprogramma's voor de validatie van een voltooide oplossing is, kunt u een nieuw pakket voor OEM testen. Wanneer de test van het pakket is verstreken, wordt het ondertekend door Microsoft. Uw test moet het bijgewerkte OEM-uitbreidingspakket met de stuurprogramma's en firmware die zijn geslaagd voor Windows Server-logo en pc's tests bevatten.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Bekijk voordat u uploaden of het verzenden van pakketten, [een OEM-pakket maken](azure-stack-vaas-create-oem-package.md) voor de indeling van de verwachte pakket en de inhoud.

## <a name="managing-packages-for-validation"></a>Beheren van pakketten voor validatie

Wanneer u de **validatie van het pakket** werkstroom voor het valideren van een pakket, moet u een URL naar een **Azure Storage-blob**. Deze blob is dat de test ondertekend OEM-pakket dat wordt geïnstalleerd als onderdeel van het updateproces. Maken van de blob met behulp van de Azure Storage-Account die u hebt gemaakt tijdens de installatie (Zie [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Voorwaarde: Inrichten van een storage-container

Maak een container in uw storage-account voor pakket-blobs. Deze container kan worden gebruikt voor al uw validatie van het pakket wordt uitgevoerd.

1. In de [Azure Portal](https://portal.azure.com), gaat u naar het storage-account hebt gemaakt in [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md).
2. Op de blade links onder **Blob-Service**selecteert op **Containers**.
3. Selecteer **+ Container** in het menu van de balk en geef een naam voor de container, bijvoorbeeld `vaaspackages`.

### <a name="upload-package-to-storage-account"></a>Pakket uploaden naar storage-account

1. Bereid het pakket dat u wilt valideren. Als het pakket meerdere bestanden heeft, comprimeren in een `.zip` bestand.
2. In de [Azure Portal](https://portal.azure.com), selecteert u de pakket-container en uploaden van het pakket door te selecteren op **uploaden** in de menubalk.
3. Selecteer het pakket `.zip` dat u wilt uploaden. Laat de standaardwaarden staan **Blobtype** (dat wil zeggen, **blok-Blob**) en **blokgrootte**.

> [!NOTE]
> Zorg ervoor dat de `.zip` inhoud worden geplaatst in de hoofdmap van de `.zip` bestand. Er mogen zich geen submappen in het pakket.

### <a name="generate-package-blob-url-for-vaas"></a>URL van het pakket blob voor VaaS genereren

Bij het maken van een **validatie van het pakket** werkstroom in de portal VaaS, moet u een URL naar de Azure Storage-blob met uw pakket opgeven.

#### <a name="option-1-generating-a-blob-sas-url"></a>Optie 1: Genereren van een blob SAS-URL

Gebruik deze optie als u niet wilt dat deze openbare leestoegang tot uw storage-container of BLOB's inschakelen.

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar uw storage-account en navigeer naar de ZIP met uw pakket

2. Selecteer **SAS genereren** in het contextmenu

3. Selecteer **lezen** van **machtigingen**

4. Instellen **begintijd** op de huidige tijd en **eindtijd** aan ten minste 48 uur vanaf **begintijd**. Als u andere tests worden uitgevoerd met hetzelfde pakket, kunt u overwegen verhogen **eindtijd** voor de lengte van de test. Alle tests gepland via VaaS na **eindtijd** zal mislukken en een nieuwe SAS moet worden gegenereerd.

5. Selecteer **Blob-SAS-token en -URL genereren**.

Gebruik de **Blob SAS-URL** wanneer URL's bieden pakket blob naar de portal.

#### <a name="option-2-grant-public-read-access"></a>Optie 2: Openbare leestoegang te verlenen

> [!CAUTION]
> Deze optie, opent u uw BLOB (s) voor anonieme toegang voor alleen-lezen.

1. Verleen **openbare leestoegang voor blobs alleen** naar de container van het pakket door de instructies in de sectie [anonieme gebruikersmachtigingen verlenen voor containers en blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

> [!NOTE]
> Als u de URL van een pakket biedt een *interactieve test* (bijvoorbeeld maandelijks AzureStack Update verificatie of verificatie van OEM-extensie-pakket), moet u verlenen **volledig openbare leestoegang** naar Ga verder met het testen.

2. Selecteer in de container pakket, de blob pakket om het eigenschappendeelvenster te openen.

3. Kopieer de **URL**. Deze waarde wordt gebruikt wanneer het pakket biedt blob-URL's naar de portal.

## <a name="apply-monthly-update"></a>Maandelijkse update van toepassing

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Een werkstroom voor validatie van het pakket maken

1. Aanmelden bij de [VaaS portal](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Selecteer **Start** op de **validatie van het pakket** tegel.

    ![Pakket validaties werkstroom tegel](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Voer de Azure Storage blob URL van de test ondertekend OEM-pakket dat een handtekening van Microsoft. Zie voor instructies [pakket blob URL genereren voor VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Omgeving parameters kunnen niet worden gewijzigd na het maken van een werkstroom.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    U wordt omgeleid naar de pagina overzicht van tests.

## <a name="required-tests"></a>Vereiste tests

Volgende tests uit zijn vereist voor validatie van de OEM-pakket:

- OEM-extensie pakket verificatie
- Engine voor cloud-simulatie

## <a name="run-package-validation-tests"></a>Pakket validatietests uit te voeren

1. In de **validatie van het pakket test samenvatting** pagina, voert u een subset van de vermelde tests die geschikt is voor uw scenario.

    In de werkstromen validatie **planning** een test maakt gebruik van het niveau van de werkstroom algemene parameters die u hebt opgegeven tijdens het maken van de werkstroom (Zie [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md)). Als een van de parameterwaarden test ongeldig geworden, moet u ze resupply volgens de instructies in [wijzigen werkstroomparameters](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

    > [!NOTE]
    > Een bestaand exemplaar van een validatietest planning maakt een nieuw exemplaar in plaats van het oude-exemplaar in de portal. Logboeken voor het oude exemplaar worden bewaard, maar zijn niet toegankelijk is vanaf de portal.  
    Wanneer een test is voltooid en de **planning** actie wordt uitgeschakeld.

2. Selecteer de agent die de test wordt uitgevoerd. Voor informatie over het toevoegen van lokale WebTest-agents worden uitgevoerd, Zie [implementeren van de lokale agent](azure-stack-vaas-local-agent.md).

3. Volledige OEM-extensie pakket verificatie selecteren **planning** in het contextmenu om een prompt voor het plannen van de test-exemplaar te openen.

4. Controleer de testparameters en selecteer vervolgens **indienen** OEM-extensie pakket verificatie plannen voor uitvoering.

5. Bekijk de resultaten voor OEM-extensie pakket verificatie. Zodra de test is geslaagd, plannen dat Cloud simulatie Engine voor uitvoering.

Wanneer alle tests zijn voltooid, stuurt u de naam van uw oplossing VaaS en validatie van het pakket naar [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) om aan te vragen pakket ondertekenen.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)