---
title: Bekende problemen en oplossingen met SCIM 2.0-protocol naleving van de Azure AD-gebruiker Provisioning-service | Microsoft Docs
description: Het oplossen van veelvoorkomende problemen met protocol compatibiliteit geconfronteerd bij het toevoegen van een toepassing buiten de galerie die ondersteuning biedt voor SCIM 2.0 naar Azure AD
services: active-directory
documentationcenter: ''
author: asmalser
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: asmalser
ms.openlocfilehash: aa7169e29ec46abc1c7de2858a118745e7363411
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53585282"
---
# <a name="known-issues-and-resolutions-with-scim-20-protocol-compliance-of-the-azure-ad-user-provisioning-service"></a>Bekende problemen en oplossingen met SCIM 2.0-protocol naleving van de Azure AD-gebruiker Provisioning-service

Azure Active Directory (Azure AD) kunt automatisch inrichten van gebruikers en groepen met elke toepassing of het systeem dat door een webservice met de interface is fronted gedefinieerd in de [systeem voor meerdere domeinen Identity Management (SCIM) 2.0-protocol specificatie](https://tools.ietf.org/html/draft-ietf-scim-api-19). 

Azure AD-ondersteuning voor het SCIM 2.0-protocol wordt beschreven in [met behulp van systeem voor meerdere domeinen Identity Management (SCIM) voor het automatisch inrichten van gebruikers en groepen uit Azure Active Directory met toepassingen](use-scim-to-provision-users-and-groups.md), waarin de specifieke onderdelen van het protocol die worden geïmplementeerd om automatisch inrichten van gebruikers en groepen uit Azure AD met toepassingen die ondersteuning bieden voor SCIM 2.0.

Dit artikel beschrijft de huidige en eerdere problemen met de Azure AD-gebruiker inrichting van de service voldoet aan het SCIM 2.0-protocol en hoe u deze problemen kunt omzeilen.

> [!IMPORTANT]
> De meest recente update naar de Azure AD gebruiker provisioning service SCIM client is uitgevoerd op 18 December 2018. Deze update gericht de bekende compatibiliteitsproblemen die worden vermeld in de onderstaande tabel. Zie de veelgestelde vragen hieronder voor meer informatie over deze update.

## <a name="scim-20-compliance-issues-and-status"></a>Problemen met de naleving van SCIM 2.0 en de status

| **Probleem met beleidsnaleving SCIM 2.0** |  **Opgelost?** | **Datum oplossen**  |  
|---|---|---|
| Azure AD is vereist ' / scim ' zich in de hoofdmap van de toepassing de SCIM eindpunt-URL  | Ja  |  18 december 2018 | 
| Extensiekenmerken gebruiken punt ". 'notatie voor de kenmerknamen van in plaats van puntkomma's ': ' de notatie |  Ja  | 18 december 2018  | 
|  Patch-aanvragen voor kenmerken met meerdere waarden bevatten filtersyntaxis ongeldig pad | Ja  |  18 december 2018  | 
|  Groep maken aanvragen bevatten een ongeldige URI-schema | Ja  |  18 december 2018  |  

## <a name="were-the-services-fixes-described-automatically-applied-to-my-pre-existing-scim-app"></a>Zijn de services-oplossingen beschreven automatisch toegepast op mijn bestaande SCIM-app?

Nee. Als dit zou een belangrijke wijziging aan SCIM-apps die zijn gecodeerd om te werken met het oudere gedrag zijn gevormd, zijn de wijzigingen niet automatisch toegepast op bestaande apps.

De wijzigingen worden toegepast op alle nieuwe [buiten de galerie SCIM apps](configure-single-sign-on-non-gallery-applications.md) geconfigureerd in Azure portal, na de datum van de oplossing.

Zie de volgende sectie voor meer informatie over het migreren van een bestaande gebruiker inrichtingstaak zodanig dat de meest recente oplossingen.

## <a name="can-i-migrate-an-existing-scim-based-user-provisioning-job-to-include-the-latest-service-fixes"></a>Kan ik een bestaande SCIM inrichten van gebruikers taak om op te nemen van de meest recente service-oplossingen migreren?

Ja. Als u al een exemplaar van deze toepassing voor eenmalige aanmelding gebruikt en de bestaande inrichtingstaak moet zodanig dat de meest recente oplossingen migreren, volgt u de onderstaande procedure. Deze procedure wordt beschreven hoe u de Microsoft Graph API en de Microsoft Graph API-Verkenner uw oude inrichtingstaak uit uw bestaande SCIM-app verwijderen en een nieuw wachtwoord maken dat het nieuwe gedrag vertoont.

> [!NOTE]
> Als uw toepassing nog in ontwikkeling is en is nog niet geïmplementeerd voor eenmalige aanmelding of gebruikers inrichten, de eenvoudigste oplossing is om te verwijderen van de toepassing-vermelding in de **Azure Active Directory > bedrijfstoepassingen**sectie van de Azure-portal, en voeg een nieuwe vermelding voor de toepassing met de **-toepassing maken > niet in de galerij** optie. Dit is een alternatief voor de onderstaande procedure uit te voeren.
 
1. Meld u aan bij Azure portal op https://portal.azure.com.
2. In de **Azure Active Directory > bedrijfstoepassingen** sectie van de Azure-portal, zoek en selecteer uw bestaande SCIM-toepassing.
3.  In de **eigenschappen** sectie van uw bestaande SCIM-app, Kopieer de **Object-ID**.
4.  Ga in een nieuw browservenster naar https://developer.microsoft.com/en-us/graph/graph-explorer en meld u aan als beheerder voor de Azure AD-tenant waar uw app wordt toegevoegd.
5. Voer de volgende opdracht te vinden van de ID van uw taak in Grafiekverkenner. "[Object-id]" vervangen door de service principal-ID (object-ID) van de derde stap gekopieerd.
 
 `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs` 

 ![Taken ophalen](./media/application-provisioning-config-problem-scim-compatibility/get-jobs.PNG "taken ophalen") 


6. Kopieer de volledige 'ID'-tekenreeks die met "customappsso" of "scim begint" in de resultaten.
7. Voer de volgende opdracht om op te halen van de configuratie van de kenmerk-koppeling, zodat u een back-up kunt maken. Gebruik dezelfde [object-id] als voordat en [taak-id] vervangen door de inrichting taak-ID in de vorige stap hebt gekopieerd.
 
 `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]/schema`
 
 ![Schema ophalen](./media/application-provisioning-config-problem-scim-compatibility/get-schema.PNG "Schema ophalen") 

8. Kopieer de JSON-uitvoer van de laatste stap en sla deze op een tekstbestand. Hierin worden eventuele aangepaste kenmerk-toewijzingen aan uw oude app toegevoegd en moet ongeveer enkele duizenden regels van JSON.
9. Voer de volgende opdracht om het inrichtingsproces taak te verwijderen:
 
 `DELETE https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]`

10. Voer de volgende opdracht om een nieuwe inrichting taak met de meest recente service-oplossingen te maken.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs `
 `{   templateId: "scim"   } `
   
11. Kopieer de volledige 'ID'-tekenreeks die met 'scim begint' in de resultaten van de laatste stap. Opnieuw toepassen (optioneel) uw oude kenmerktoewijzingen met de opdracht hieronder en vervang [nieuwe-taak-id] met de nieuwe taak-ID die u zojuist hebt gekopieerd en het invoeren van die de JSON-uitvoer van stap #7 als hoofdtekst van de aanvraag.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[new-job-id]/schema `
 `{   <your-schema-json-here>   }`

12. Ga terug naar het browservenster en selecteer de **Provisioning** tabblad voor uw toepassing.
13. Controleer de configuratie en start de taak. 

## <a name="can-i-add-a-new-non-gallery-app-that-has-the-old-user-provisioning-behavior"></a>Kan ik een nieuwe buiten de galerie-app met de oude gebruikersinrichting gedrag toevoegen?

Ja. Als u de oude gedrag die bestonden voordat de oplossingen en moet een nieuw exemplaar van het implementeren van een toepassing had gecodeerd, volgt u de onderstaande procedure. Deze procedure wordt beschreven hoe u de Microsoft Graph API en de API van Microsoft Graph explorer gebruiken om u te maken van een SCIM de taak die de oude gedrag vertoont.
 
1.  Meld u aan bij Azure portal op https://portal.azure.com.
2. in de **Azure Active Directory > bedrijfstoepassingen >-toepassing maken** sectie van de Azure-portal, maakt u een nieuw **niet in de galerij** toepassing.
3.  In de **eigenschappen** sectie van de nieuwe aangepaste app kopiëren de **Object-ID**.
4.  Ga in een nieuw browservenster naar https://developer.microsoft.com/en-us/graph/graph-explorer en meld u aan als beheerder voor de Azure AD-tenant waar uw app wordt toegevoegd.
5. Voer de volgende opdracht om te initialiseren van de inrichtingsconfiguratie voor uw app in Grafiekverkenner.
"[Object-id]" vervangen door de service principal-ID (object-ID) van de derde stap gekopieerd.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
 `{   templateId: "customappsso"   }`
 
6. Ga terug naar het browservenster en selecteer de **Provisioning** tabblad voor uw toepassing.
7. Voltooi de gebruikersinrichting configureren zoals u gewend bent.


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de inrichting en ongedaan maken inrichting voor SaaS-toepassingen](user-provisioning.md)

