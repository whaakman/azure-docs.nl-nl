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
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c2b0343ff472fe380750152712ca88d9ebb404e2
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57782782"
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

1. In de [Azure-portal](https://portal.azure.com), gaat u naar het storage-account hebt gemaakt in [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md).

2. Op de blade links onder **Blob-Service**, selecteer **Containers**.

3. Selecteer **+ Container** in de menubalk.
    1. Geef een naam op voor de container, bijvoorbeeld `vaaspackages`.
    1. Selecteer de gewenste toegangsniveau voor niet-geverifieerde clients zoals VaaS. Zie voor meer informatie over het verlenen van toegang tot pakketten in elk scenario VaaS [afhandeling van container-toegangsniveau](#handling-container-access-level).

### <a name="upload-package-to-storage-account"></a>Pakket uploaden naar storage-account

1. Bereid het pakket dat u wilt valideren. Dit is een `.zip` bestand waarvan de inhoud moeten overeenkomen met de structuur wordt beschreven in [maken van een OEM-pakket](azure-stack-vaas-create-oem-package.md).

    > [!NOTE]
    > Zorg ervoor dat de `.zip` inhoud worden geplaatst in de hoofdmap van de `.zip` bestand. Er mogen zich geen submappen in het pakket.

1. In de [Azure-portal](https://portal.azure.com), selecteert u de pakket-container en uploaden van het pakket door te selecteren op **uploaden** in de menubalk.

1. Selecteer het pakket `.zip` dat u wilt uploaden. Laat de standaardwaarden staan **Blobtype** (dat wil zeggen, **blok-Blob**) en **blokgrootte**.

### <a name="generate-package-blob-url-for-vaas"></a>URL van het pakket blob voor VaaS genereren

Bij het maken van een **validatie van het pakket** werkstroom in de portal VaaS, moet u een URL naar de Azure Storage-blob met uw pakket opgeven. Sommige *interactieve* tests, met inbegrip van **maandelijkse AzureStack Update verificatie** en **OEM-extensie pakket verificatie**, een URL naar pakket blobs zijn ook vereist.

#### <a name="handling-container-access-level"></a>Toegangsniveau voor afhandeling van container

Het minimale toegangsniveau vereist voor VaaS is afhankelijk van of u een werkstroom voor validatie van het pakket wordt gemaakt of plannen van een *interactieve* testen.

In het geval van **persoonlijke** en **Blob** toegangsniveaus, u moet tijdelijk toegang verlenen tot de blob pakket door middel van een VaaS een [handtekening voor gedeelde toegang](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1?) (SAS). De **Container** toegangsniveau vereist niet dat u voor het genereren van SAS-URL's, maar niet-geverifieerde toegang tot de container en de blobs.

|Toegangsniveau | Werkstroom-vereiste | Vereiste test |
|---|---------|---------|
|Privé | Genereren van een SAS-URL per pakket blob ([optie 1](#option-1-generate-a-blob-sas-url)). | Genereren van een SAS-URL op accountniveau en handmatig toevoegen van de blob-naam van het pakket ([optie 2](#option-2-construct-a-container-sas-url)). |
|Blob | Geef de eigenschap van de blob-URL ([optie 3](#option-3-grant-public-read-access)). | Genereren van een SAS-URL op accountniveau en handmatig toevoegen van de blob-naam van het pakket ([optie 2](#option-2-construct-a-container-sas-url)). |
|Container | Geef de eigenschap van de blob-URL ([optie 3](#option-3-grant-public-read-access)). | Geef de eigenschap van de blob-URL ([optie 3](#option-3-grant-public-read-access)).

De opties voor het verlenen van toegang tot uw pakketten zijn gerangschikt op de minimale toegang grootste toegang krijgen tot.

#### <a name="option-1-generate-a-blob-sas-url"></a>Optie 1: Genereren van een blob SAS-URL

Gebruik deze optie als u het toegangsniveau van uw container voor opslag is ingesteld op **persoonlijke**, waar de container openbare leestoegang tot de container en blobs niet inschakelen.

> [!NOTE]
> Deze methode werkt niet voor *interactieve* tests. Zie [optie 2: Maken van een container SAS-URL](#option-2-construct-a-container-sas-url).

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar uw storage-account en navigeer naar de ZIP met uw pakket.

2. Selecteer **SAS genereren** in het contextmenu.

3. Selecteer **lezen** van **machtigingen**.

4. Instellen **begintijd** op de huidige tijd en **eindtijd** aan ten minste 48 uur vanaf **begintijd**. Als u andere werkstromen worden maken met hetzelfde pakket, kunt u overwegen verhogen **eindtijd** voor de lengte van de test.

5. Selecteer **Blob-SAS-token en -URL genereren**.

Gebruik de **Blob SAS-URL** wanneer URL's bieden pakket blob naar de portal.

#### <a name="option-2-construct-a-container-sas-url"></a>Optie 2: Maken van een container SAS-URL

Gebruik deze optie als u het toegangsniveau van uw container voor opslag is ingesteld op **persoonlijke** en moet u opgeven van een pakket met blob-URL naar een *interactieve* testen. Deze URL kan ook worden gebruikt op het werkstroomniveau van de.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Selecteer **Blob** van **toegestane Services opties**. Schakel alle overige opties uit.

1. Selecteer **Container** en **Object** van **toegestane resourcetypen**.

1. Selecteer **lezen** en **lijst** van **toegestane machtigingen**. Schakel alle overige opties uit.

1. Selecteer **begintijd** als de huidige tijd en **eindtijd** aan ten minste veertien dagen vanaf de **begintijd**. Als u andere tests worden uitgevoerd met hetzelfde pakket, kunt u overwegen verhogen **eindtijd** voor de lengte van de test. Alle tests gepland via VaaS na **eindtijd** zal mislukken en een nieuwe SAS moet worden gegenereerd.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    De indeling moet er als volgt uitzien: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Wijzigen van de gegenereerde SAS-URL om op te nemen van de container pakket `{containername}`, en de naam van uw blob pakket `{mypackage.zip}`, als volgt:  `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Deze waarde wordt gebruikt wanneer het pakket biedt blob-URL's naar de portal.

#### <a name="option-3-grant-public-read-access"></a>Optie 3: Openbare leestoegang te verlenen

Gebruik deze optie als het is mogelijk om toegang tot afzonderlijke blobs of, in het geval van niet-geverifieerde clients toestaan *interactieve* tests, de container.

> [!CAUTION]
> Deze optie, opent u uw BLOB (s) voor anonieme toegang voor alleen-lezen.

1. Stel het toegangsniveau van de container pakket **Blob** of **Container** door de instructies in de sectie [anonieme gebruikersmachtigingen verlenen voor containers en blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

    > [!NOTE]
    > Als u de URL van een pakket biedt een *interactieve* test, moet u verlenen **volledig openbare leestoegang** naar de container om door te gaan met het testen.

1. Selecteer in de container pakket, de blob pakket om het eigenschappendeelvenster te openen.

1. Kopieer de **URL**. Deze waarde wordt gebruikt wanneer het pakket biedt blob-URL's naar de portal.

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

De volgende tests uit zijn vereist voor validatie van de OEM-pakket:

- OEM-extensie pakket verificatie
- Engine voor cloud-simulatie

## <a name="run-package-validation-tests"></a>Pakket validatietests uit te voeren

1. In de **validatie van het pakket test samenvatting** pagina, voert u een subset van de vermelde tests die geschikt is voor uw scenario.

    In de werkstromen validatie **planning** een test maakt gebruik van het niveau van de werkstroom algemene parameters die u hebt opgegeven tijdens het maken van de werkstroom (Zie [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md)). Als een van de parameterwaarden test ongeldig geworden, moet u ze resupply volgens de instructies in [wijzigen werkstroomparameters](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

    > [!NOTE]
    > Een bestaand exemplaar van een validatietest planning maakt een nieuw exemplaar in plaats van het oude-exemplaar in de portal. Logboeken voor het oude exemplaar worden bewaard, maar zijn niet toegankelijk is vanaf de portal.  
    > Wanneer een test is voltooid en de **planning** actie wordt uitgeschakeld.

2. Selecteer de agent die de test wordt uitgevoerd. Voor informatie over het toevoegen van lokale WebTest-agents worden uitgevoerd, Zie [implementeren van de lokale agent](azure-stack-vaas-local-agent.md).

3. OEM-extensie-pakket om verificatie te voltooien, selecteert u **planning** in het contextmenu om een prompt voor het plannen van de test-exemplaar te openen.

4. Controleer de testparameters en selecteer vervolgens **indienen** OEM-extensie pakket verificatie plannen voor uitvoering.

    OEM-extensie pakket verificatie is opgesplitst in twee stappen: Azure Stack-Update en OEM-Update.

    1. **Selecteer** 'Uitvoeren' in de gebruikersinterface van de controle vooraf script wilt uitvoeren. Dit is een geautomatiseerde test die duurt ongeveer 5 minuten om te voltooien en is geen actie vereist.

    1. Als de controle vooraf script is voltooid, de handmatige stap uitvoeren: **installeren** de meest recente beschikbare Azure Stack update met behulp van de Azure Stack-portal.

    1. **Voer** Test-AzureStack op het stempel. Als er fouten optreden, ga niet verder met de test- en neem contact op met [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

        Zie voor meer informatie over het uitvoeren van de opdracht Test-AzureStack [valideren Azure Stack-systeemstatus](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test).

    1. **Selecteer** 'Volgende' om uit te voeren van het postcheck script. Dit is een geautomatiseerde test en markeert het einde van het updateproces voor de Azure Stack.

    1. **Selecteer** "Uitvoeren" om uit te voeren van het script controle vooraf voor OEM-Update.

    1. Als de controle is voltooid, de handmatige stap uitvoeren: **installeren** het OEM-uitbreidingspakket via de portal.

    1. **Voer** Test-AzureStack op het stempel.

        > [!NOTE]
        > Als voorheen, ga niet verder met de test- en neem contact op met [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) als dit mislukt. Deze stap is kritiek omdat u bovendien een opnieuw implementeren bespaart.

    1. **Selecteer** 'Volgende' om uit te voeren van het postcheck script. Hiermee markeert het einde van de OEM-update-stap.

    1. Alle resterende beantwoorden aan het einde van de test en **Selecteer** "Verzenden".

    1. Hiermee markeert het einde van de interactieve test.

5. Bekijk de resultaten voor OEM-extensie pakket verificatie. Zodra de test is geslaagd, plannen dat Cloud simulatie Engine voor uitvoering.

Als u wilt een pakket aanvraag ondertekenen, verzenden [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) de naam van oplossing en de naam van de validatie van het pakket die zijn gekoppeld aan deze uitvoering.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)
