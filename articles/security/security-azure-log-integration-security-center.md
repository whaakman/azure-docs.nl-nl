---
title: Integratie van Azure logboek met security center | Microsoft Docs
description: Meer informatie over het Azure Security center alerts werken met logboek-integratie
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
ms.date: 08/29/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 9acce21d544a43adcd0c0983771c02c6bb39caec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-your-security-center-alerts-in-azure-log-integration"></a>Het ophalen van uw Security Center-waarschuwingen in de Azure-logboekanalyse-integratie

Dit artikel bevat de stappen die nodig is voor de integratie van Azure Log-service voor het ophalen van informatie over beveiliging waarschuwingen gegenereerd door Azure Security Center. U moet hebt voltooid de stappen in de [aan de slag](security-azure-log-integration-get-started.md) artikel voordat u de stappen in dit artikel uitvoert.

## <a name="detailed-steps"></a>Gedetailleerde stappen

De volgende stappen wordt de vereiste Azure Active Directory Service-Principal maken en toewijzen van de Service Principle leesrechten heeft voor het abonnement:
1. Open de opdrachtprompt en navigeer naar **c:\Program Files\Microsoft Azure Log-integratie**
2. Voer de opdracht``azlog createazureid``

    Met deze opdracht wordt u gevraagd om uw Azure-aanmelding. De opdracht maakt u vervolgens een [Azure Active Directory Service-Principal](../active-directory/develop/active-directory-application-objects.md) in de Azure AD-Tenants die als host fungeren van de Azure-abonnementen waarin de aangemelde gebruiker een beheerder, CO-beheerder of een eigenaar is. De opdracht mislukt als de aangemelde gebruiker een gastgebruiker in de Azure AD-Tenant. Verificatie in Azure wordt uitgevoerd via Azure Active Directory (AD). Maken van een service-principal voor Azlog integratie maakt de identiteit van de Azure AD dat toegang tot het lezen van de Azure-abonnementen wordt verleend.

3. Vervolgens voert u een opdracht waarmee leestoegang hebben voor het abonnement worden toegewezen aan de service-principal die u zojuist hebt gemaakt. Als u een abonnements-id niet opgeeft, wordt de opdracht geprobeerd toewijzen van de rol van service principal lezer alle abonnementen op die u geen toegang hebben hebt. </br></br>
``azlog authorize <SubscriptionID>`` </br> bijvoorbeeld </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Mogelijk ziet u waarschuwingen als u de opdracht autoriseren onmiddellijk nadat de ```createazureid``` opdracht. Er is enige vertraging worden bijgewerkt tussen wanneer het Azure AD-account wordt gemaakt en wanneer het account is beschikbaar voor gebruik. Als u ongeveer 60 seconden worden voltooid nadat u wacht de ```createazureid``` opdracht de opdracht autoriseren, u moet deze waarschuwingen niet zien.

4. Controleer de volgende mappen om te bevestigen dat de Audit log JSON-bestanden er zijn:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Controleer de volgende mappen om te bevestigen dat Security Center-waarschuwingen in deze bestaat:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Als u problemen ondervindt wordt uitgevoerd tijdens de installatie en configuratie, opent u een [ondersteuningsaanvraag](/azure-supportability/how-to-create-azure-support-request.md), selecteer **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor meer informatie over de integratie van Azure-logboek:

* [Microsoft Azure Log-integratie voor Azure logboeken](https://www.microsoft.com/download/details.aspx?id=53324) : Download Center voor meer informatie over de systeemvereisten en instructies op Azure-logboekanalyse-integratie installeren.
* [Inleiding tot Azure-logboekanalyse integratie](security-azure-log-integration-overview.md) : dit document bevat een inleiding tot Azure-logboekanalyse integratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u de integratie van Azure-logboekanalyse werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.
* [Azure-logboekanalyse integratie Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md) -deze Veelgestelde vragen over de antwoorden op vragen over Azure-logboekanalyse-integratie.
* [Nieuwe functies voor Azure diagnoses en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) : dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies waarmee u inzicht in de bewerkingen van uw Azure-resources.
