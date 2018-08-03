---
title: Datacenter-integratie Azure Stack - beveiliging
description: Meer informatie over het integreren van Azure Stack-beveiliging met de datacenterbeveiliging van uw
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: 9f356b814ac1ac6ca8b6d6efe7cb9f5d9ed66270
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442472"
---
# <a name="azure-stack-datacenter-integration---security"></a>Datacenter-integratie Azure Stack - beveiliging
Azure Stack is ontworpen en gebouwd met beveiliging in gedachten. Azure Stack is een systeem vergrendeld, zodat de installatie van de agent van software wordt niet ondersteund.

Dit artikel helpt u de beveiligingsfuncties van Azure Stack te integreren met de beveiligingsoplossingen die al zijn geïmplementeerd in uw datacenter.

## <a name="security-logs"></a>Beveiligingslogboeken

Azure Stack verzamelt besturingssysteem en beveiligingsgebeurtenissen voor de functies van de infrastructuur en schaal eenheid knooppunten elke twee minuten. De logboeken worden opgeslagen in de storage-account blob-containers.

Er is één opslagaccount per functie van de infrastructuur en een algemeen opslagaccount voor alle typisch besturingssysteemgebeurtenissen.

De health-resourceprovider kan worden aangeroepen via de REST-protocol om op te halen van de URL naar de blob-container. Beveiligingsoplossingen van derden kunnen de API- en storage-accounts gebruiken voor het ophalen van gebeurtenissen voor verwerking.

### <a name="use-azure-storage-explorer-to-view-events"></a>Azure Storage Explorer gebruiken om gebeurtenissen te bekijken

U kunt gebeurtenissen die worden verzameld door Azure Stack is een hulpprogramma voor Azure Storage Explorer met de naam ophalen. U kunt Azure Storage Explorer uit downloaden [ http://storageexplorer.com ](http://storageexplorer.com).

De volgende procedure is een voorbeeld die kunt u Azure Storage Explorer configureren voor Azure Stack:

1. Meld u aan de portal van Azure Stack-beheerder als een operator.
1. Blader **opslagaccounts** en zoek naar **frphealthaccount**. De **frphealthaccount** is het algemene opslagaccount dat is gebruikt voor het opslaan van alle gebeurtenissen van het besturingssysteem.

   ![Opslagaccounts](media/azure-stack-integrate-security/storage-accounts.png)

1. Selecteer **frphealthaccount**, klikt u vervolgens op **toegangssleutels**.

   ![Toegangssleutels](media/azure-stack-integrate-security/access-keys.png)

1. De toegangssleutel naar het Klembord kopiëren.
1. Open Azure Storage Explorer.
1. Op de **bewerken** in het menu **Azure Stack als doel**.
1. Selecteer **-Account toevoegen**, en selecteer vervolgens **een opslagaccountnaam en -sleutel gebruiken**.

   ![Verbinding maken met opslag](media/azure-stack-integrate-security/connect-storage.png)

1. Klik op **Volgende**.
1. Op de **externe opslag koppelen** pagina:

   a. Typ de naam van het **frphealthaccount**.

   b. De toegangssleutel voor opslagaccount plakken.

   c. Onder **domein van opslageindpunten**, selecteer **andere**, en geeft u het opslageindpunt **[regio]. [ Domeinnaam]**.

   d. Selecteer de **Gebruik HTTP** selectievakje.

   ![Externe opslag koppelen](media/azure-stack-integrate-security/attach-storage.png)

1. Klik op **volgende**, controleert u de samenvatting en **voltooien** de Wizard.
1. U kunt nu de afzonderlijke blobcontainers bladeren en downloaden van de gebeurtenissen.

   ![Door blobs bladeren](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Gebruik van moderne programmeertalen op toegangsgebeurtenissen

U kunt diverse programmeertalen gebruiken voor toegang tot een opslagaccount. Gebruik de volgende documentatie voor een voorbeeld dat overeenkomt met de taal kiezen:

[https://azure.microsoft.com/resources/samples/?term=storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Controle van apparaat

Alle fysieke apparaten in Azure Stack ondersteuning voor het gebruik van TACACS of RADIUS. Dit omvat toegang tot de BMC (baseboard management controller) en de netwerkswitches.

Azure Stack-oplossingen niet geleverd met RADIUS of TACACS ingebouwd. De oplossingen zijn echter gevalideerd ter ondersteuning van het gebruik van bestaande RADIUS of TACACS oplossingen beschikbaar in de markt.

Voor RADIUS, is MSCHAPv2 gevalideerd. Hiermee wordt de veiligste implementatie met behulp van RADIUS.
Neem contact op met uw hardwareleverancier OEM TACAS of RADIUS inschakelen in de apparaten die zijn opgenomen in uw Azure Stack-oplossing.

## <a name="syslog"></a>Syslog

Alle fysieke apparaten in Azure Stack kunnen Syslog-berichten verzenden. Azure Stack-oplossingen niet geleverd met een Syslog-server. De oplossingen zijn echter gevalideerd ter ondersteuning van berichten verzenden naar bestaande Syslog oplossingen beschikbaar in de markt.

Het doeladres Syslog is een optionele parameter voor de implementatie kan worden verzameld, maar deze kan ook worden toegevoegd na de implementatie.

## <a name="next-steps"></a>Volgende stappen

[Servicebeleid](azure-stack-servicing-policy.md)
