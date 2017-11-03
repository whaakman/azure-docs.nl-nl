---
title: Integratie met Azure Active Directory-auditlogboeken van Azure logboek | Microsoft Docs
description: Informatie over het installeren van de integratie van Azure Log-service en het integreren van Logboeken van Azure controlelogboeken
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 8a1295cc86057ed72940e774d0bd423d61142e31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integratie van Azure Active Directory-auditlogboeken

Controlegebeurtenissen van Azure Active Directory (Azure AD) kunnen u bepalen bevoorrechte acties die is opgetreden in Azure Active Directory. U kunt zien welke typen gebeurtenissen die u bijhouden aan de hand van kunt [Azure Active Directory-controlerapportgebeurtenissen](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).

> [!NOTE]
> Voordat u de stappen in dit artikel, moet u controleren de [aan de slag](security-azure-log-integration-get-started.md) en voltooi de stappen bevat.

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Stappen voor het integreren van Azure Active directory-auditlogboeken

1. Open de opdrachtprompt en voer deze opdracht uit:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Voer deze opdracht uit: 
 
   ``azlog createazureid``

   Met deze opdracht wordt u gevraagd om uw Azure-aanmelding. De opdracht maakt u een Azure Active Directory service-principal in de Azure AD-tenants die als host fungeren van de Azure-abonnementen waarin de aangemelde gebruiker een beheerder, CO-beheerder of een eigenaar is. De opdracht mislukt als de aangemelde gebruiker een gastgebruiker in de Azure AD-tenant. Verificatie in Azure wordt gedaan door Azure AD. Maken van een service-principal voor Azure Log integratie maakt de identiteit van de Azure AD die toegang krijgt tot het lezen van de Azure-abonnementen.

3. Voer de volgende opdracht om uw tenant-ID. U moet lid zijn van de tenant-beheerdersrol de opdracht uit te voeren.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Voorbeeld:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Controleer de volgende mappen om te bevestigen dat de Azure Active Directory audit log JSON-bestanden in deze zijn gemaakt:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

De volgende video ziet u de stappen die in dit artikel:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Neem contact op met de leverancier van uw SIEM voor specifieke instructies op de informatie in de JSON-bestanden te brengen in uw security information en event management (SIEM)-systeem.

Hulp is beschikbaar via de [Azure Log integratie MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Dit forum kunt mensen in de Azure Log integratie community ter ondersteuning van elkaar met vragen, antwoorden, tips en slagen. Bovendien het team van Azure Log integratie dit forum wordt bewaakt en helpt wanneer dat mogelijk is.

U kunt ook openen een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de integratie van Azure-logboek:

* [Microsoft Azure Log-integratie voor Azure logboeken](https://www.microsoft.com/download/details.aspx?id=53324): deze Download Center-pagina geeft details, systeemvereisten en installatie-instructies voor de integratie van Azure-logboek.
* [Inleiding tot Azure Log integratie](security-azure-log-integration-overview.md): in dit artikel bevat een inleiding tot Azure Log integratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): dit blogbericht ziet u de integratie van Azure Log werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.
* [Veelgestelde vragen over de Azure Log integratie](security-azure-log-integration-faq.md): in dit artikel antwoorden op vragen over Azure Log-integratie.
* [Security Center waarschuwingen integreren met Azure Log integratie](../security-center/security-center-integrating-alerts-with-log-integration.md): dit artikel laat zien hoe u wilt synchroniseren van waarschuwingen van Beveiligingscentrum, samen met de beveiliging van de virtuele machine gebeurtenissen die worden verzameld door Azure Diagnostics- en Azure audit logboeken met uw logboekanalyse of SIEM-oplossing.
* [Nieuwe functies voor Azure Diagnostics- en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies waarmee u inzicht in de bewerkingen van uw Azure-resources.
