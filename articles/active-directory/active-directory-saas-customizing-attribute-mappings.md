---
title: Azure AD-kenmerktoewijzingen aanpassen | Microsoft Docs
description: Meer informatie over welke kenmerktoewijzingen voor SaaS-apps in Azure Active Directory zijn hoe u kunt deze aanpassen om de behoeften van uw bedrijf op te lossen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ca2fdc9c68ea0030d938eeaebd57aafa0e2790f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Gebruikers inrichten kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory aanpassen
Microsoft Azure AD biedt ondersteuning voor gebruikersinrichting voor SaaS-toepassingen van derden zoals Salesforce en Google Apps. Als er gebruikers inrichten voor een SaaS-toepassing van derden is ingeschakeld, bepaalt de Azure-beheerportal de kenmerkwaarden weer in de vorm van een configuratie met de naam "kenmerk wordt toegewezen."

Er is een set vooraf geconfigureerde kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten van elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen of contactpersonen. <br> 
 U kunt de standaard-kenmerktoewijzingen aanpassen volgens uw bedrijfsbehoeften. Dit houdt in dat u kunt wijzigen of verwijderen van bestaande kenmerktoewijzingen of nieuwe kenmerktoewijzingen maken.

In de Azure AD-portal, opent u deze functie door te klikken op een **toewijzingen** configuratie onder **inrichten** in de **beheren** gedeelte van een **bedrijfstoepassing**.


![SalesForce][5] 

Te klikken op een **toewijzingen** configuratie, de verwante geopend **kenmerk toewijzing** blade.  
Er zijn kenmerktoewijzingen die door een SaaS-toepassing vereist zijn te laten functioneren. Voor de vereiste kenmerken de **verwijderen** functie is niet beschikbaar.


![SalesForce][6]  

In het bovenstaande voorbeeld kunt u zien dat de **gebruikersnaam** kenmerk van een beheerd object in Salesforce is gevuld met de **userPrincipalName** waarde van de gekoppelde Azure Active Directory-Object.

U kunt bestaande **kenmerktoewijzingen** door te klikken op een toewijzing. Hiermee opent u de **kenmerk bewerken** blade.

![SalesForce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Kenmerk typen toewijzing
Met kenmerktoewijzingen, kunt u bepalen hoe kenmerken worden ingevuld in een SaaS-toepassing van derden. Er zijn vier verschillende toewijzingstypen ondersteund:

* **Directe** : het kenmerk target is gevuld met de waarde van een kenmerk van het gekoppelde object in Azure AD.
* **Constante** : het kenmerk target is gevuld met een specifieke tekenreeks die u hebt opgegeven.
* **Expressie** -het kenmerk target is ingevuld op basis van het resultaat van een script-achtige-expressie. 
  Zie voor meer informatie [schrijven expressies voor kenmerktoewijzingen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Geen** -het kenmerk target blijft ongewijzigd. Echter, als het kenmerk target ooit leeg is, wordt dit ingevuld met de standaardwaarde die u opgeeft.

Naast deze vier eenvoudige kenmerk toewijzingstypen aangepaste kenmerktoewijzingen ondersteuning voor het concept van een optionele **standaard** waarde toewijzing. De toewijzing van de standaard waarde zorgt ervoor dat een target-kenmerk is gevuld met een waarde als er geen waarde in Azure AD en ook op het doelobject. De meest voorkomende configuratie is leeg laten.


## <a name="understanding-attribute-mapping-properties"></a>Understanding toewijzing kenmerkeigenschappen

In de vorige sectie hebt u al is kennis met de eigenschap type van kenmerk toewijzing.
Naast deze eigenschap ondersteunen kenmerktoewijzingen ook de volgende kenmerken:

- **Bronkenmerk** -gebruikerskenmerk uit het bronsysteem (bijvoorbeeld: Azure Active Directory).
- **Doelkenmerk** – het gebruikerskenmerk in het doelsysteem (bijvoorbeeld: ServiceNow).
- **Overeen met objecten met behulp van dit kenmerk** : of deze toewijzing moet worden gebruikt als unieke identificatie van gebruikers tussen de bron en doel-systemen of niet. Dit is standaard ingesteld op het kenmerk userPrincipalName of e-mail in Azure AD, die meestal naar een veld username in een doeltoepassing is toegewezen.
- **Overeenkomende voorrang** – meerdere overeenkomende kenmerken kan worden ingesteld. Wanneer er meerdere, moeten ze worden geëvalueerd in de volgorde gedefinieerd door dit veld. Als een overeenkomst is gevonden, er is geen verdere overeenkomende kenmerken worden geëvalueerd.
- **Deze toewijzing is van toepassing**
    - **Altijd** : deze toewijzing is van toepassing op beide maken van een gebruikersaccount en acties bijwerken
    - **Alleen tijdens het maken van** -deze toewijzing is van toepassing alleen op gebruikersacties maken


## <a name="what-you-should-know"></a>Wat u moet weten

Microsoft Azure AD biedt een efficiënte implementatie van een synchronisatieproces. In een omgeving met geïnitialiseerde worden alleen objecten die moet worden bijgewerkt verwerkt tijdens een synchronisatiecyclus. Kenmerktoewijzingen bijwerken, heeft een invloed op de prestaties van een synchronisatiecyclus. Een update voor de configuratie van de toewijzing is vereist voor alle beheerde objecten die opnieuw worden geëvalueerd. Het is aanbevolen om het aantal opeenvolgende wijzigingen tot uw kenmerktoewijzingen minimaal te beperken.

## <a name="next-steps"></a>Volgende stappen

* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Automatisch gebruikers inrichten/opheffen van inrichting tot SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](active-directory-scim-provisioning.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Meldingen inrichten van een account](active-directory-saas-account-provisioning-notifications.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

