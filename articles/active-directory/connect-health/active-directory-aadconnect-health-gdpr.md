---
title: Azure AD Connect Health en algemene gegevens beveiliging regelgeving | Microsoft Docs
description: Dit document beschrijft het verkrijgen van GDPR compliancy met Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2018
ms.author: billmath
ms.openlocfilehash: b9a0b9027bbead00300040186e453933b3a7f46b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect-health"></a>GDPR compatibiliteit en Azure AD Connect Health 

[Algemene gegevens beveiliging regelgeving (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) is van een Europese Unie beveiliging en privacy-wet. De GDPR legt nieuwe regels voor bedrijven, overheidsinstanties, non-profitorganisaties en andere organisaties die waren en diensten aan te aan mensen in de EU of die bieden verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. 

Microsoft-producten en services zijn vandaag beschikbaar waarmee u voldoen aan de vereisten GDPR. Meer informatie over het privacybeleid van Microsoft voor op [Vertrouwenscentrum](https://www.microsoft.com/trustcenter).

Azure AD Connect Health controleert uw lokale-identity-service-infrastructuur en synchronisatie. Dit biedt u ook inzicht en verwerkingsinformatie waarschuwingen. Microsoft hecht GDPR-naleving van alle cloudservices wanneer het afdwingen van mei 2018 wordt gestart en GDPR-gerelateerde garantie in haar contractueel verplichtingen. 

>[!NOTE] 
> In dit artikel bevat een overzicht van de naleving van de GDPR in Azure AD Connect Health. Zie voor informatie over de naleving van de GDPR in Azure AD Connect, [GDPR naleving en Azure AD Connect](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="gdpr-classification"></a>GDPR classificatie
Azure AD Connect Health worden onderverdeeld in de **gegevensprocessor** categorie van GDPR classificatie. De service biedt services voor gegevensverwerking een pijplijn gegevensprocessor tot belangrijke partners en end consumenten. Azure AD Connect Health genereert geen gebruikersgegevens en heeft geen onafhankelijke controle over welke persoonlijke gegevens worden verzameld en hoe deze wordt gebruikt. Gegevens ophalen, aggregatie, analyse en rapportage in Azure AD Connect Health zijn gebaseerd op bestaande on-premises gegevens. 

## <a name="data-retention-policy"></a>Beleid voor gegevensretentie
Azure AD Connect Health geen rapporten genereren, analyses uitvoeren of inzicht dan 30 dagen. Daarom Azure AD Connect Health niet opslaan, verwerkt of alle gegevens na 30 dagen bewaren. Dit ontwerp is compatibel met de GDPR voorschriften, Microsoft privacy nalevingsreglementen en bewaarbeleid voor gegevens van Azure AD. 
 
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
Zie [verwijderen van een server van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#to-delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Uitschakelen van gegevensverzameling en -bewaking voor een exemplaar van een bewaakte service
Zie [verwijderen van een service-exemplaar van Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Opnieuw inschakelen van gegevensverzameling en bewaken in Azure AD Connect Health
Als opnieuw wilt inschakelen in Azure AD Connect Health voor een eerder verwijderde bewaakte service bewaken, moet u verwijderen en [opnieuw installeren van de health-agent](active-directory-aadconnect-health-agent-install.md) op alle servers.


## <a name="next-steps"></a>Volgende stappen
* [Het beleid voor Microsoft Privacy op Vertrouwenscentrum bekijken](https://www.microsoft.com/trustcenter)
* [Azure AD Connect en GDPR](../../active-directory/connect/active-directory-aadconnect-gdpr.md)
* [Azure AD Connect Health-bewerkingen](active-directory-aadconnect-health-operations.md)
