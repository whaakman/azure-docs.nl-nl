---
title: Waarschuwingen van Azure Security Center integreren met Azure-logboekanalyse-integratie | Microsoft Docs
description: In dit artikel helpt u aan de slag met Security Center waarschuwingen integreren met Azure-logboekanalyse-integratie.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: terrylan
ms.openlocfilehash: d13e5b87c446e587091551b22d80fe568d5d8093
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Waarschuwingen van Azure Security Center integreren met Azure-logboekanalyse-integratie
Veel beveiligingsbewerkingen en respons op incidenten teams afhankelijk van een oplossing Security Information en Event Management (SIEM) als uitgangspunt voor gesorteerd en beveiligingswaarschuwingen onderzoeken. U kunt waarschuwingen van Azure Security Center met uw SIEM-oplossing integreren met Azure Log-integratie.

Azure-logboekanalyse-integratie ondersteunt momenteel HP ArcSight Splunk en IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik worden geïntegreerd?
Azure produceert uitgebreide logboekregistratie in voor elke service. Deze logboeken zijn aangemerkt als:

* **Besturingselement/Management-logboeken** die geven inzicht in de Azure Resource Manager maken, bijwerken en DELETE-bewerkingen. Deze controlevlak gebeurtenissen worden opgehaald in de logboeken van de Azure-activiteit
* **Gegevenslogboeken vlak** die geven inzicht in de gebeurtenissen die deze gebeurtenis treedt op wanneer u een Azure-resource. Een voorbeeld is het Windows-gebeurtenislogboek, waar u de beveiligingsgegevens voor de gebeurtenis van de Event Viewer-beveiligingskanaal kunt krijgen. Data vlak-gebeurtenissen (die worden gegenereerd door een virtuele machine of een Azure-service) worden opgehaald door Azure diagnostische logboeken.

Azure-logboekanalyse-integratie ondersteunt momenteel de integratie van:

* Azure VM-Logboeken
* Azure controlelogboeken
* Azure Security Center-waarschuwingen

## <a name="install-azure-log-integration"></a>Azure-logboekanalyse-integratie installeren
Download [integratie van Azure-logboekanalyse](https://www.microsoft.com/download/details.aspx?id=53324).

De Azure-logboekanalyse integration-service verzamelt telemetriegegevens van de computer waarop deze is geïnstalleerd.  Verzamelde telemetriegegevens is:

* Uitzonderingen die tijdens het uitvoeren van de integratie van Azure-logboekanalyse optreden
* Metrische gegevens over het aantal query's en gebeurtenissen die zijn verwerkt
* Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

> [!NOTE]
> U kunt verzamelen van telemetriegegevens uitschakelen door deze optie uitschakelt.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integratie van Azure controlelogboeken en Security Center-waarschuwingen
1. Open de opdrachtprompt en **cd** in **c:\Program Files\Microsoft Azure Log integratie**.
2. Voer de **azlog createazureid** opdracht maken een [Azure Active Directory Service-Principal](../active-directory/active-directory-application-objects.md) in de Azure Active Directory (AD)-tenants die de Azure-abonnementen te hosten.

    U wordt gevraagd naar uw Azure-aanmelding.

   > [!NOTE]
   > U moet de eigenaar van het abonnement of Medebeheerder van het abonnement.
   >
   >

    Verificatie in Azure wordt gedaan door Azure AD.  Maken van een service-principal voor Azure-logboekanalyse integratie maakt de identiteit van de Azure AD die toegang krijgt tot het lezen van de Azure-abonnementen.
3. Voer de **azlog autoriseren <SubscriptionID>**  opdracht leestoegang hebben voor het abonnement toewijzen aan de service-principal gemaakt in stap 2. Als u geen opgeeft een **SubscriptionID**, wordt de service-principal met de rol lezer is toegewezen aan alle abonnementen waartoe u toegang hebt.

   > [!NOTE]
   > Mogelijk ziet u waarschuwingen als u de **autoriseren** opdracht onmiddellijk na de **createazureid** opdracht. Er is enige vertraging worden bijgewerkt tussen wanneer het Azure AD-account wordt gemaakt en wanneer het account is beschikbaar voor gebruik. Als u 10 seconden worden voltooid nadat u wacht de **createazureid** uit te voeren opdracht de **autoriseren** uitvoert, en u moet deze waarschuwingen niet zien.
   >
   >
4. Controleer de volgende mappen om te bevestigen dat de Audit log JSON-bestanden er zijn:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Controleer de volgende mappen om te bevestigen dat Security Center-waarschuwingen in deze bestaat:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. De SIEM-bestand doorstuurserver connector configureren zodat de juiste map. De procedure variëren, afhankelijk van de SIEM die u gebruikt.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure activiteitenlogboeken en eigenschapdefinities:

* [Bewerkingen controleren met Resource Manager](../azure-resource-manager/resource-group-audit.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) — Lees het laatste nieuws van de Azure-beveiliging en de informatie.
