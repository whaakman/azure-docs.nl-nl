---
title: Azure-Logboekintegratie met Azure Active Directory-controlelogboeken | Microsoft Docs
description: Meer informatie over het installeren van de service Azure-Logboekintegratie en logboeken van Azure-auditlogboeken integreren
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 473f9db7eb507fa910657696590bb656a8aff52f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109874"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Controlelogboeken van Azure Active Directory integreren

Controlegebeurtenissen van Azure Active Directory (Azure AD) kunnen u bevoegde acties die zijn opgetreden in Azure Active Directory identificeren. U kunt zien welke typen gebeurtenissen die u aan de hand kunt houden [Active Directory van Azure-controlegebeurtenissen rapport](../active-directory/reports-monitoring/concept-audit-logs.md).


>[!IMPORTANT]
> De functie van de integratie met Azure Log worden afgeschaft door 06/01/2019. AzLog downloads zijn uitgeschakeld op 27 juni 2018. Voor informatie over wat te doen verplaatsen forward revisie van het bericht [gebruikt Azure monitor om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Stappen voor het integreren van Azure Active Directory-auditlogboeken

> [!NOTE]
> Voordat u de stappen in dit artikel, moet u controleren de [aan de slag](security-azure-log-integration-get-started.md) en voltooi de relevante stappen er.

1. Open de opdrachtprompt en voer deze opdracht uit:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Voer deze opdracht uit: 
 
   ``azlog createazureid``

   Met deze opdracht vraagt u om uw Azure-aanmelding. De opdracht maakt u een Azure Active Directory service-principal in de Azure AD-tenants die als host fungeren de waarin zich de aangemelde gebruiker een beheerder, CO-beheerder of eigenaar van een Azure-abonnementen. De opdracht mislukt als de gebruiker aangemeld alleen een gastgebruiker in de Azure AD-tenant is. Azure-verificatie wordt gedaan via Azure AD. Een service-principal voor Azure-Logboekintegratie maakt, wordt de Azure AD-identiteit die toegang krijgt tot het lezen van de Azure-abonnementen.

3. Voer de volgende opdracht uit voor uw tenant-ID. U moet lid zijn van de rol van tenant-beheerder de opdracht uit te voeren.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Voorbeeld:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Controleer de volgende mappen om te bevestigen dat de Azure Active Directory audit log JSON-bestanden worden gemaakt in deze:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

De volgende video ziet u de stappen in dit artikel besproken:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Neem contact op met de leverancier van uw SIEM voor specifieke instructies over hoe u de informatie in de JSON-bestanden in uw beveiligingsgegevens en event management (SIEM) system brengt.

Hulp is beschikbaar via de [integratie van MSDN-Forum van Azure Log](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Dit forum kunt mensen in de Azure-Logboekintegratie community ter ondersteuning van elkaar met vragen, antwoorden, tips en trucs. Bovendien het team van Azure-Logboekintegratie dit forum wordt bewaakt en helpt wanneer dat mogelijk is.

U kunt ook openen een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer **Logboekintegratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-Logboekintegratie:

* [Microsoft Azure-Logboekintegratie voor Azure-logboeken](https://www.microsoft.com/download/details.aspx?id=53324): Deze pagina Download Center geeft details, systeemvereisten en installatie-instructies voor Azure-Logboekintegratie.
* [Inleiding tot Azure-Logboekintegratie](security-azure-log-integration-overview.md): In dit artikel vindt u Azure-Logboekintegratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Azure-Logboekintegratie Veelgestelde vragen over](security-azure-log-integration-faq.md): In dit artikel vindt u antwoorden op vragen over Azure-Logboekintegratie.
* [Nieuwe functies voor Azure Diagnostics en Azure-auditlogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Dit blogbericht bevat een inleiding tot Azure-auditlogboeken en andere functies waarmee u inzicht in de bewerkingen van uw Azure-resources.
