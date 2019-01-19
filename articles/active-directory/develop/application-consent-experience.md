---
title: Inzicht krijgen in Azure AD-toepassing toestemming ervaringen | Microsoft Docs
description: Meer dat informatie over de Azure AD toestemming om te zien hoe u het kunt gebruiken bij het beheren en ontwikkelen van toepassingen in Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: zawad
ms.openlocfilehash: 3193e9ac6ac5e90584bbf548142b7573d61c310a
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413070"
---
# <a name="understanding-azure-ad-application-consent-experiences"></a>Inzicht krijgen in Azure AD-toepassing toestemming ervaringen

Meer dat informatie over de Azure Active Directory (Azure AD)-toepassing toestemming geven gebruikerservaring. Zodat u kunt op intelligente wijze beheren van toepassingen voor uw organisatie en/of toepassingen met een meer naadloze ervaring voor toestemming ontwikkelen.

## <a name="consent-and-permissions"></a>Machtigingen en toestemming

Toestemming is het proces van een gebruiker toestemming om een toepassing te krijgen tot beveiligde bronnen namens hen te verlenen. Een beheerder of gebruiker vragen om toestemming voor toegang tot de gegevens van hun organisatie/persoon.

De ervaring van de werkelijke gebruiker toestemming verlenen zullen verschillen afhankelijk van de beleidsregels die zijn ingesteld op de tenant van de gebruiker van de gebruiker bereik van-instantie (of rol) en het type [machtigingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-permissions) wordt aangevraagd door de clienttoepassing. Dit betekent dat ontwikkelaars van toepassingen en tenantbeheerders kunnen een controle over de toestemming hebben. Beheerders hebben de flexibiliteit van instellen en het uitschakelen van beleidsregels op een tenant of de app voor het beheren van de ervaring toestemming in hun tenant. Ontwikkelaars van toepassingen kunnen bepalen welke typen machtigingen worden aangevraagd en als ze willen gebruikers begeleidt bij de gebruiker toestemming stroom of de beheerder toestemming stroom.

- **Gebruikersstroom toestemming** is als een ontwikkelaar van de toepassing zorgt ervoor gebruikers op het autorisatie-eindpunt met de bedoeling om te registreren toestemming voor alleen de huidige gebruiker dat.
- **Beheerder instemmingsstroom** is als een ontwikkelaar van de toepassing zorgt ervoor dat gebruikers door de beheerder toestemming geven-eindpunt met de bedoeling om te registreren toestemming voor de gehele tenant. Als u wilt controleren of de beheerder toestemming stroom correct werkt, ontwikkelaars van toepassingen moeten lijst met alle machtigingen in de `RequiredResourceAccess` eigenschap in het toepassingsmanifest. Zie voor meer informatie, [toepassingsmanifest](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest).

## <a name="building-blocks-of-the-consent-prompt"></a>Bouwstenen van de toestemming vragen

De toestemmingsprompt is ontworpen om te controleren of gebruikers beschikken over voldoende informatie om te bepalen of ze de clienttoepassing toegang krijgen tot beveiligde bronnen namens hen vertrouwen. Informatie over de bouwstenen, kunnen gebruikers toestemming zorg verlenen beter geïnformeerde beslissingen te nemen en het helpt ontwikkelaars betere gebruikerservaringen bouwen.

Het volgende diagram en de tabel bevatten informatie over de bouwstenen bij het vragen om toestemming.

![Bouwstenen van de toestemming vragen](./media/application-consent-experience/consent_prompt.png)

| # | Onderdeel | Doel |
| ----- | ----- | ----- |
| 1 | Gebruikers-id | Deze id vertegenwoordigt de gebruiker die toegang krijgen tot beveiligde bronnen namens de clienttoepassing vraagt. |
| 2 | Titel | De titel wordt gewijzigd op basis van of de gebruikers gaan door de gebruiker of beheerder toestemming-stroom. In een gebruikersstroom toestemming zijn de titel 'Machtigingen aangevraagd' in de beheerstroom toestemming van de titel moet een extra regel 'Accepteren voor uw organisatie'. |
| 3 | App-logo | Deze afbeelding kunt gebruikers hebben een visuele aanwijzing van of deze app de app is ze bedoeld voor toegang tot. Deze installatiekopie wordt geleverd door de ontwikkelaars van toepassingen en het eigendom van deze installatiekopie is niet gevalideerd. |
| 4 | Naam van app | Deze waarde moet in kennis stellen gebruikers welke toepassing toegang tot hun gegevens aanvraagt. Noteer deze naam wordt geleverd door de ontwikkelaars en het eigendom van de naam van deze app is niet gevalideerd. |
| 5 | Uitgeversdomein | Deze waarde moet aangeboden aan gebruikers met een domein dat ze mogelijk is om te evalueren voor betrouwbaarheid. Dit domein wordt geleverd door de ontwikkelaars en het eigendom van dit domein publisher is gevalideerd. |
| 6 | Machtigingen | Deze lijst bevat de machtigingen die zijn aangevraagd door de clienttoepassing. Gebruikers moeten altijd evalueren voor de typen machtigingen worden aangevraagd om te begrijpen welke gegevens de clienttoepassing wordt worden gemachtigd voor toegang tot namens hen als ze accepteren. Als toepassingsontwikkelaar in een is het raadzaam om toegang te vragen, met de machtigingen met minimale bevoegdheden. |
| 7 | Beschrijving van machtiging | Deze waarde is opgegeven door de service de machtigingen om vrij te geven. Als u wilt zien van de beschrijving van de machtigingen, moet u de pijl-omlaag naast de machtiging schakelen. |
| 8 | App-voorwaarden | Deze termen bevatten koppelingen naar de gebruiksvoorwaarden en privacyverklaring van de toepassing. De uitgever is verantwoordelijk voor het geeft een overzicht van de regels in de servicevoorwaarden. Daarnaast is de uitgever die verantwoordelijk is voor de vermelding van de manier waarop ze gebruiken en delen van gegevens van de gebruiker in de privacyverklaring. Als de uitgever geen koppelingen naar deze waarden voor toepassingen met meerdere tenants biedt, wordt er een waarschuwing vet weergegeven bij de toestemmingsprompt. |
| 9 | https://myapps.microsoft.com | Dit is de koppeling waarop gebruikers kunnen bekijken en verwijderen van alle niet-Microsoft-toepassingen die momenteel toegang tot hun gegevens hebben. |

## <a name="common-consent-scenarios"></a>Algemene scenario's voor toestemming

Hier volgen de ervaring toestemming van een gebruiker in de algemene toestemming-scenario's zien kan:

1. Personen toegang tot een app die ze omgeleid naar de gebruiker toestemming stroom tijdens het vereisen van een machtigingenset dat zich binnen hun bereik van de autorisaties.
    
    1. Beheerders zien een extra controle op de traditionele toestemmingsprompt waarmee ze toestemming namens de gehele tenant. Het besturingselement wordt standaard ingesteld op uitgeschakeld, dus alleen namens de gehele tenant wanneer beheerders expliciet controleren op het vak wordt toestemming worden verleend. Vanaf vandaag, wordt dit selectievakje alleen weergegeven voor de rol globale beheerder, zodat de beheerder van de Cloud en beheer-App niet zichtbaar voor dit selectievakje in.

        ![Toestemming vragen om scenario 1a](./media/application-consent-experience/consent_prompt_1a.png)
    
    2. Gebruikers zien de traditionele toestemmingsprompt.

        ![Toestemming vragen om scenario 1b](./media/application-consent-experience/consent_prompt_1b.png)

2. Personen toegang tot een app waarvoor ten minste één machtiging die zich buiten het bereik van de autorisaties.
    1. Beheerders zien de dezelfde opdrachtprompt als 1.i hierboven wordt weergegeven.
    2. Gebruikers krijgen geen toegang verlenen tot de toepassing en deze wordt gemeld dat hun beheerder vragen om toegang tot de app. 
                
        ![Toestemming vragen om scenario 1b](./media/application-consent-experience/consent_prompt_2b.png)

3. Personen die navigeren of worden doorgestuurd naar de beheerder toestemming stroom.
    1. Beheerder gebruikers zien de beheerder toestemming gevraagd. De titel en de beschrijvingen van de machtiging gewijzigd op deze vraag, de wijzigingen markeren van het feit dat de app worden verleend als u een daarmee toegang tot de aangevraagde gegevens namens de gehele tenant.
        
        ![Toestemming vragen om scenario 1b](./media/application-consent-experience/consent_prompt_3a.png)
        
    1. Gebruikers zonder beheerdersrechten ziet hetzelfde scherm als 2. ii hierboven wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen
- Een stapsgewijze overzicht van [hoe de Azure AD-toestemmingsframework toestemming implementeert](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).
- Lees voor meer diepgang [hoe het toestemmingsframework kunt gebruiken in een toepassing met meerdere tenants](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) implementeren 'gebruiker' en 'admin' toestemming, ondersteunt meer geavanceerde toepassingen met meerdere lagen.
