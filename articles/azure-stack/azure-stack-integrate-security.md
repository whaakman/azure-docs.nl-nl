---
title: Stack datacenter integratie van Azure - beveiliging
description: Meer informatie over het integreren van Azure-Stack-beveiliging met de beveiliging van uw datacenter
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/17/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: eb7c651362838d44d6558e080e6130b4a8041d1e
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---security"></a>Stack datacenter integratie van Azure - beveiliging

*Van toepassing op: Azure Stack geïntegreerd systemen*

Azure-Stack is ontwikkeld en gebouwd met beveiligd zijn. Azure-Stack is een systeem vergrendeld, zodat de installatie van de agent van software wordt niet ondersteund.

In dit artikel helpt u bij Azure-Stack beveiligingsfuncties integreren met de beveiligingsoplossingen die al zijn geïmplementeerd in uw datacenter.

## <a name="security-logs"></a>Beveiligingslogboeken

Azure Stack verzamelt besturingssysteem en beveiligingsgebeurtenissen voor infrastructuur-rollen en scale unit knooppunten elke twee minuten. De logboeken worden opgeslagen in blob-containers voor storage-account.

Er is een opslagaccount per infrastructuurrol en een algemeen opslagaccount voor alle gebeurtenissen van de typische besturingssysteem.

De health-resourceprovider kan worden aangeroepen via de REST-protocol voor het ophalen van de URL van de blob-container. Oplossingen van derden kunnen de API en storage-accounts gebruiken voor het ophalen van gebeurtenissen voor verwerking.

### <a name="use-azure-storage-explorer-to-view-events"></a>Azure Storage Explorer gebruiken om gebeurtenissen te bekijken

U kunt de verzamelde gebeurtenissen per Azure-Stack met een hulpprogramma voor Azure Storage Explorer ophalen. U kunt Azure Storage Explorer uit downloaden [http://storageexplorer.com](http://storageexplorer.com).

De volgende procedure is een voorbeeld die kunt u Azure Storage Explorer configureren voor Azure-Stack:

1. Aanmelden bij de Azure-Stack-beheerdersportal als operator.
2. Blader **opslagaccounts** en zoekt u **frphealthaccount**. De **frphealthaccount** is de algemene opslagaccount gebruikt voor het opslaan van alle gebeurtenissen van het besturingssysteem.

   ![Opslagaccounts](media/azure-stack-integrate-security/storage-accounts.png)

3. Selecteer **frphealthaccount**, klikt u vervolgens op **toegangstoetsen**.

   ![Toegangssleutels](media/azure-stack-integrate-security/access-keys.png)

4. De toegangssleutel naar het Klembord kopiëren.
5. Open Azure Opslagverkenner.
6. Op de **bewerken** selecteert u **doel Azure Stack**.
7. Selecteer **Account toevoegen**, en selecteer vervolgens **gebruik van een naam van het opslagaccount en de sleutel**.

   ![Verbinding maken met opslag](media/azure-stack-integrate-security/connect-storage.png)

8. Klik op **Volgende**.
9. Op de **externe opslag koppelen** pagina:

   a. Typ de naam van de account **frphealthaccount**.

   b. Plak de toegangssleutel voor opslagaccount.

   c. Onder **opslag eindpunten domein**, selecteer **andere**, en geeft u het eindpunt opslag **[regio]. [ Domeinnaam]**.

   d. Selecteer de **HTTP gebruiken** selectievakje.

   ![Externe opslag koppelen](media/azure-stack-integrate-security/attach-storage.png)

10. Klik op **volgende**, controleert u de samenvatting en **voltooien** de Wizard.
11. U kunt nu de afzonderlijke blob-containers bladeren en downloaden van de gebeurtenissen.

   ![BLOB's bladeren](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Programmeertalen toegang gebeurtenissen gebruiken

U kunt verschillende programmeertalen gebruiken voor toegang tot een opslagaccount. Gebruik de volgende documentatie voor een voorbeeld die overeenkomt met de taal kiezen:

[https://Azure.Microsoft.com/resources/Samples/?term=Storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Controle van apparaat

Alle fysieke apparaten in Azure-Stack ondersteuning voor het gebruik van TACACS of RADIUS. Het gaat hierbij om toegang tot de BMC (baseboard management controller) en de netwerkswitches.

Azure Stack-oplossingen niet geleverd met RADIUS of TACACS ingebouwd. De oplossingen zijn echter gevalideerd ter ondersteuning van het gebruik van bestaande RADIUS of TACACS oplossingen beschikbaar in de markt.

Voor RADIUS, is MSCHAPv2 gevalideerd. Hiermee wordt de veiligste implementatie met behulp van RADIUS.
Neem contact op met uw hardwareleverancier OEM TACAS of RADIUS inschakelen in de apparaten die deel uitmaakt van uw Azure-Stack-oplossing.

## <a name="syslog"></a>Syslog

Alle fysieke apparaten in Azure-Stack kunnen Syslog-berichten verzenden. Azure Stack-oplossingen niet geleverd met een Syslog-server. De oplossingen zijn echter gevalideerd ter ondersteuning van berichten verzenden naar bestaande Syslog-oplossingen beschikbaar in de markt.

Het doeladres Syslog is een optionele parameter verzameld voor de implementatie, maar kan ook worden toegevoegd na implementatie.

## <a name="next-steps"></a>Volgende stappen

[Azure datacenter integratie Stack - eindpunten publiceren](azure-stack-integrate-endpoints.md)
