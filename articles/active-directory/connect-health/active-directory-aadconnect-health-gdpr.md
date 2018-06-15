---
title: Azure AD Connect Health en Privacy van gebruikers | Microsoft Docs
description: Dit document beschrijft de privacy van gebruikers met Azure AD Connect Health.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: 5fedbac439636b56da217e7babd30820bce7b342
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33931755"
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Privacy van gebruikers en Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>In dit artikel gaat over Azure AD Connect Health en gebruiker privacy.  Zie het artikel voor informatie over Azure AD Connect en gebruiker privacy [hier](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="user-privacy-classification"></a>Gebruiker privacy classificatie
Azure AD Connect Health worden onderverdeeld in de **gegevensprocessor** categorie van GDPR classificatie. De service biedt services voor gegevensverwerking een pijplijn gegevensprocessor tot belangrijke partners en end consumenten. Azure AD Connect Health genereert geen gebruikersgegevens en heeft geen onafhankelijke controle over welke persoonlijke gegevens worden verzameld en hoe deze wordt gebruikt. Gegevens ophalen, aggregatie, analyse en rapportage in Azure AD Connect Health zijn gebaseerd op bestaande on-premises gegevens. 

## <a name="data-retention-policy"></a>Beleid voor gegevensretentie
Azure AD Connect Health geen rapporten genereren, analyses uitvoeren of inzicht dan 30 dagen. Daarom Azure AD Connect Health niet opslaan, verwerkt of alle gegevens na 30 dagen bewaren. Dit ontwerp is compatibel met de GDPR voorschriften, Microsoft privacy nalevingsreglementen en bewaarbeleid voor gegevens van Azure AD. 

Servers met actieve **Health service-gegevens is niet up-to-date** **fout** waarschuwingen voor meer dan 30 opeenvolgende dagen raden aan dat er geen gegevens Connect Health heeft bereikt tijdens die periode. Deze servers wordt uitgeschakeld en wordt niet weergegeven in de portal Connect Health. Als u wilt de servers opnieuw inschakelt, moet u verwijderen en [opnieuw installeren van de health-agent](active-directory-aadconnect-health-agent-install.md). Houd er rekening mee dat dit niet van toepassing op **waarschuwingen** met hetzelfde Waarschuwingstype. Waarschuwingen geven aan dat gegevens gedeeltelijk ontbreken op de server die u wordt gewaarschuwd voor. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Gegevens verzamelen en bewaken in Azure AD Connect Health uitschakelen
Azure AD Connect Health kunt u stop het verzamelen van gegevens voor elke bewaakte server of voor een exemplaar van een bewaakte service. U kunt bijvoorbeeld gegevens te verzamelen voor afzonderlijke servers voor ADFS (Active Directory Federation Services) die worden bewaakt met Azure AD Connect Health stoppen. U kunt ook het verzamelen van gegevens voor het hele AD FS-exemplaar dat wordt bewaakt met Azure AD Connect Health stoppen. Wanneer u hiervoor kiest, worden de bijbehorende servers via de portal Azure AD Connect Health verwijderd na het stoppen van verzamelen van gegevens. 

>[!IMPORTANT]
> U moet hoofdbeheerder voor Azure AD-bevoegdheden of de rol Inzender in RBAC verwijderen bewaakte servers van Azure AD Connect Health.
>
> Verwijderen van een server of de service-exemplaar van Azure AD Connect Health is niet een omkeerbare actie. 

### <a name="what-to-expect"></a>Wat u kunt verwachten?
Als u de gegevensverzameling en -bewaking voor een afzonderlijke bewaakte server of een exemplaar van een bewaakte service stopt, moet u rekening houden met het volgende:

- Wanneer u een exemplaar van een bewaakte service verwijdert, wordt het exemplaar wordt verwijderd uit de Azure AD Connect Health monitoring servicelijst in de portal. 
- Wanneer u een bewaakte server of een exemplaar van een bewaakte service verwijdert, wordt de Health-Agent niet verwijderd of verwijderd van uw servers. De Health-Agent is geconfigureerd niet om gegevens te verzenden naar Azure AD Connect Health. U moet handmatig verwijderen van de Health-Agent op eerder bewaakte servers.
- Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk foutgebeurtenissen op de server (s) die betrekking hebben op de Health-Agent.
- Alle gegevens die behoren tot het exemplaar van de bewaakte service is verwijderd volgens het Gegevensretentiebeleid van Microsoft Azure.

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Uitschakelen van gegevensverzameling en -bewaking voor een exemplaar van een bewaakte service
Zie [verwijderen van een service-exemplaar van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Gegevensverzameling en -bewaking voor een bewaakte server uitschakelen
Zie [verwijderen van een server van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Uitschakelen van gegevensverzameling en -bewaking van alle bewaakte services in Azure AD Connect Health
Azure AD Connect Health biedt ook de optie voor het verzamelen van gegevens van stoppen **alle** services geregistreerd in de tenant. U wordt aangeraden zorgvuldig en volledige bevestiging van alle globale beheerders voordat de actie te ondernemen. Zodra het proces wordt gestart, stopt Connect Health-service ontvangen, verwerken en de rapportage van alle gegevens van alle services. Bestaande gegevens in het Connect Health-service niet meer dan 30 dagen bewaard.
Als u stop het verzamelen van gegevens van specifieke server wilt, volg de stappen in de verwijdering van specifieke servers. Om te stoppen tenant-wise gegevensverzameling, volgt u de volgende stappen voor het verzamelen van gegevens stoppen en verwijderen van alle services van de tenant.

1.  Klik op **algemene instellingen** onder configuratie in de hoofdblade. 
2.  Klik op **gegevens verzamelen stoppen** knop boven aan de blade. De andere opties van de tenant-instellingen wordt uitgeschakeld wanneer het proces wordt gestart.  
 
 ![Stop het verzamelen van gegevens](./media/active-directory-aadconnect-health-gdpr/gdpr4.png)
  
3.  Zorg ervoor dat de lijst met vrijgegeven services die worden beïnvloed door het verzamelen van gegevens wordt gestopt. 
4.  Voer de naam van de exacte tenant zodat de **verwijderen** actieknop
5.  Klik op **verwijderen** voor het activeren van het verwijderen van alle services. Connect Health stopt ontvangen, verwerken en de rapportage-gegevens verzonden van uw services vrijgegeven. Het volledige proces kan 24 uur duren. U ziet deze stap is niet omkeerbaar. 
6.  Nadat het proces is voltooid, ziet u alle geregistreerde services in Connect Health meer. 

 ![Nadat het verzamelen van gegevens is gestopt](./media/active-directory-aadconnect-health-gdpr/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Opnieuw inschakelen van gegevensverzameling en bewaken in Azure AD Connect Health
Als opnieuw wilt inschakelen in Azure AD Connect Health voor een eerder verwijderde bewaakte service bewaken, moet u verwijderen en [opnieuw installeren van de health-agent](active-directory-aadconnect-health-agent-install.md) op alle servers.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>Opnieuw inschakelen van gegevensverzameling en -bewaking van alle bewaakte services

Tenant-Wise gegevensverzameling kan worden hervat in Azure AD Connect Health. U wordt aangeraden zorgvuldig en volledige bevestiging van alle globale beheerders voordat de actie te ondernemen.

>[!IMPORTANT]
> De volgende stappen zijn beschikbaar nadat de 24 uur van de actie uitschakelen.
> Na het inschakelen van verzamelen van gegevens, worden de aangeboden inzicht en bewakingsgegevens in Connect Health niet weergegeven verouderde gegevens die worden verzameld voordat. 

1.  Klik op **algemene instellingen** onder configuratie in de hoofdblade. 
2.  Klik op **gegevensverzameling inschakelen** knop boven aan de blade. 
 
 ![Gegevensverzameling inschakelen](./media/active-directory-aadconnect-health-gdpr/gdpr6.png)
 
3.  Voer de naam van de exacte tenant voor het activeren van de **inschakelen** knop.
4.  Klik op **inschakelen** knop machtigen van verzamelen van gegevens in het Connect Health-service. De wijziging wordt binnenkort worden toegepast. 
5.  Ga als volgt de [installatieproces](active-directory-aadconnect-health-agent-install.md) opnieuw installeren van de agent op de servers moeten worden bewaakt en de services aanwezig zijn in de portal.  


## <a name="next-steps"></a>Volgende stappen
* [Het beleid voor Microsoft Privacy op Vertrouwenscentrum bekijken](https://www.microsoft.com/trustcenter)
* [Azure AD Connect en Privacy van gebruikers](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

