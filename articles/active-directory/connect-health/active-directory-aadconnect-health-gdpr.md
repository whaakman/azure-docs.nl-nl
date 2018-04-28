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
ms.openlocfilehash: bf26e91308cfec0dc8ede20e683919b5764a4868
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
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

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Gegevensverzameling en -bewaking voor een bewaakte server uitschakelen
Zie [verwijderen van een server van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Uitschakelen van gegevensverzameling en -bewaking voor een exemplaar van een bewaakte service
Zie [verwijderen van een service-exemplaar van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Opnieuw inschakelen van gegevensverzameling en bewaken in Azure AD Connect Health
Als opnieuw wilt inschakelen in Azure AD Connect Health voor een eerder verwijderde bewaakte service bewaken, moet u verwijderen en [opnieuw installeren van de health-agent](active-directory-aadconnect-health-agent-install.md) op alle servers.


## <a name="next-steps"></a>Volgende stappen
* [Het beleid voor Microsoft Privacy op Vertrouwenscentrum bekijken](https://www.microsoft.com/trustcenter)
* [Azure AD Connect en Privacy van gebruikers](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

