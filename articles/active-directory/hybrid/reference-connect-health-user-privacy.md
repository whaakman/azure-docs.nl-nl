---
title: Azure AD Connect Health en Privacy van gebruikers | Microsoft Docs
description: Dit document beschrijft de privacy van gebruikers met Azure AD Connect Health.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0a7dc5152bb1ede5f7311e74815a6d58782d32a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199320"
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Gebruikersprivacy en Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>In dit artikel behandelt de Azure AD Connect Health en gebruiker privacy.  Zie het artikel voor meer informatie over Azure AD Connect en gebruiker privacy [hier](reference-connect-user-privacy.md).

## <a name="user-privacy-classification"></a>Gebruiker privacy classificatie
Azure AD Connect Health valt in de **gegevensverwerker** categorie van GDPR-classificatie. Als een pijplijn gegevensverwerker biedt de service services voor gegevensverwerking op belangrijke partners en klanten te beëindigen. Azure AD Connect Health genereert geen gebruikersgegevens en heeft geen onafhankelijke controle over welke persoonlijke gegevens worden verzameld en hoe deze wordt gebruikt. Het ophalen van gegevens, aggregatie, analyse en rapportage in Azure AD Connect Health zijn gebaseerd op bestaande on-premises gegevens. 

## <a name="data-retention-policy"></a>Beleid voor gegevensretentie
Azure AD Connect Health rapporten, analyses uitvoeren, of geen inzichten langer dan 30 dagen te kunnen bieden. Daarom Azure AD Connect Health niet opslaan, verwerken, of alle gegevens langer dan 30 dagen bewaren. Dit ontwerp is compatibel met de GDPR voorschriften, Microsoft privacy de regelgeving en bewaarbeleid voor Azure AD-gegevens. 

Servers met actieve **Health service-gegevens is niet up-to-date** **fout** waarschuwingen voor meer dan 30 opeenvolgende dagen voorstellen dat er geen gegevens Connect Health tijdens die periode is bereikt. Deze servers wordt uitgeschakeld en wordt niet weergegeven in Connect Health-portal. Als u wilt de servers opnieuw inschakelt, moet u eerst verwijderen en [opnieuw installeren van de health-agent](how-to-connect-health-agent-install.md). Houd er rekening mee dat dit niet van toepassing op **waarschuwingen** met hetzelfde Waarschuwingstype. Waarschuwingen geven aan dat gegevens gedeeltelijk in de server die u wordt gewaarschuwd ontbreekt voor. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Uitschakelen van gegevensverzameling en -bewaking in Azure AD Connect Health
Azure AD Connect Health kunt u het verzamelen van gegevens voor elke bewaakte server of voor een exemplaar van een bewaakte service stoppen. Bijvoorbeeld, kunt u het verzamelen van gegevens voor afzonderlijke ADFS (Active Directory Federation Services)-servers die worden bewaakt met behulp van Azure AD Connect Health stoppen. U kunt ook gegevens verzamelen voor het hele AD FS-exemplaar dat wordt bewaakt met behulp van Azure AD Connect Health stoppen. Als u ervoor kiest om dit te doen, worden de bijbehorende servers na het stoppen van het verzamelen van gegevens vanuit de Azure AD Connect Health-portal, verwijderd. 

>[!IMPORTANT]
> U moet globale beheerder van Azure AD-bevoegdheden of de rol Inzender in RBAC verwijderen bewaakt servers van Azure AD Connect Health.
>
> Verwijderen van een server of service-exemplaar van Azure AD Connect Health is niet een actie ongedaan worden gemaakt. 

### <a name="what-to-expect"></a>Wat u kunt verwachten?
Als u het verzamelen van gegevens en -bewaking voor een afzonderlijke bewaakte server of een exemplaar van een bewaakte service stopt, Let op het volgende:

- Wanneer u een exemplaar van een bewaakte service verwijdert, wordt het exemplaar wordt verwijderd uit de Azure AD Connect Health monitoring servicelijst in de portal. 
- Als u een bewaakte server of een exemplaar van een bewaakte service verwijdert, wordt de Health-Agent niet verwijderd of verwijderd van uw servers. De Health-Agent is geconfigureerd niet voor het verzenden van gegevens naar Azure AD Connect Health. U moet handmatig verwijderen van de Health-Agent op de eerder bewaakte servers.
- Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk gebeurtenissen op foutniveau op de server (s) met betrekking tot de Health-Agent.
- Alle gegevens die behoren tot het exemplaar van de bewaakte service is verwijderd volgens het Gegevensretentiebeleid van Microsoft Azure.

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Gegevensverzameling en -bewaking voor een exemplaar van een bewaakte service uitschakelen
Zie [over het verwijderen van een service-exemplaar van Azure AD Connect Health](how-to-connect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Gegevensverzameling en -bewaking voor een bewaakte server uitschakelen
Zie [hoe u een server verwijderen uit Azure AD Connect Health](how-to-connect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Uitschakelen van gegevensverzameling en -bewaking van alle bewaakte services in Azure AD Connect Health
Azure AD Connect Health biedt ook de optie voor het verzamelen van gegevens van stoppen **alle** services geregistreerd in de tenant. U wordt aangeraden een zorgvuldige afweging en volledige geeft u aan dat alle globale beheerders voordat de actie wordt ondernomen. Zodra het proces wordt gestart, worden de Connect Health-service wordt gestopt ontvangen, verwerkt en rapporteert geen gegevens van alle services. Bestaande gegevens in Connect Health-service wordt niet meer dan 30 dagen worden bewaard.
Als u verzamelen van gegevens van specifieke server stoppen wilt, volgt u de stappen bij het verwijderen van specifieke servers. Als u wilt stoppen tenant-wise gegevensverzameling, volgt u de volgende stappen uit voor het verzamelen van gegevens stoppen en verwijderen van alle services van de tenant.

1.  Klik op **algemene instellingen** onder configuratie in de hoofdblade. 
2.  Klik op **gegevensverzameling stoppen** knop boven aan de blade. De andere opties van de tenant-instellingen worden, uitgeschakeld nadat het proces wordt gestart.  
 
 ![Stop het verzamelen van gegevens](./media/reference-connect-health-user-privacy/gdpr4.png)
  
3.  Zorg ervoor dat de lijst met onboarding-services die worden beïnvloed door het verzamelen van gegevens gestopt. 
4.  Voer de naam van de exacte tenant om in te schakelen de **verwijderen** actieknop
5.  Klik op **verwijderen** voor het activeren van het verwijderen van alle services. Verbinding maken met Health stopt ontvangen, verwerken, rapporteert geen gegevens verzonden vanuit uw onboarding-services. Het hele proces van kan tot 24 uur duren. U ziet deze stap is niet ongedaan worden gemaakt. 
6.  Nadat het proces is voltooid, wordt u geen geregistreerde services in Connect Health meer ziet. 

 ![Nadat het verzamelen van gegevens gestopt](./media/reference-connect-health-user-privacy/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Opnieuw inschakelen van gegevensverzameling en -bewaking in Azure AD Connect Health
Als u wilt opnieuw inschakelen bewaking in Azure AD Connect Health voor een eerder verwijderde bewaakte service, moet u eerst verwijderen en [opnieuw installeren van de health-agent](how-to-connect-health-agent-install.md) op alle servers.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>Opnieuw inschakelen van gegevensverzameling en -bewaking van alle bewaakte services

Tenant-Wise gegevensverzameling kan worden hervat in Azure AD Connect Health. U wordt aangeraden een zorgvuldige afweging en volledige geeft u aan dat alle globale beheerders voordat de actie wordt ondernomen.

>[!IMPORTANT]
> De volgende stappen zijn beschikbaar na 24 uur van de actie uitschakelen.
> Na het inschakelen van verzamelen van gegevens, worden het gepresenteerde inzicht en bewakingsgegevens in Connect Health niet weergegeven verouderde gegevens die worden verzameld voordat. 

1.  Klik op **algemene instellingen** onder configuratie in de hoofdblade. 
2.  Klik op **gegevensverzameling inschakelen** knop boven aan de blade. 
 
 ![Gegevensverzameling inschakelen](./media/reference-connect-health-user-privacy/gdpr6.png)
 
3.  Voer de naam van de exacte tenant activeren de **inschakelen** knop.
4.  Klik op **inschakelen** knop toestemming van de gegevensverzameling in Connect Health-service te verlenen. De wijziging wordt over enkele ogenblikken worden toegepast. 
5.  Ga als volgt de [installatieproces](how-to-connect-health-agent-install.md) opnieuw installeren van de agent op de servers moeten worden bewaakt en de services aanwezig zijn in de portal.  


## <a name="next-steps"></a>Volgende stappen
* [De Microsoft Privacy controlebeleid op Vertrouwenscentrum](https://www.microsoft.com/trustcenter)
* [Azure AD Connect en Privacy van gebruikers](reference-connect-user-privacy.md)

