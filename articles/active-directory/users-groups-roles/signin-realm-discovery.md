---
title: Opzoeken van de gebruikersnaam tijdens de aanmelding bij verificatie - Azure Active Directory | Microsoft Docs
description: Hoe op het scherm messaging gebruikersnaam lookup weergegeven tijdens het aanmelden
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/03/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91d0dbaf9b648f42ef535d8b491df04b2c7042d7
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007516"
---
# <a name="home-realm-discovery-during-sign-in-for-microsoft-365-services"></a>Thuisdomein detectie tijdens het aanmelden voor Microsoft 365-services

We wijzigen onze Azure Active Directory (Azure AD) aanmelding gedrag om te scheppen voor nieuwe verificatiemethoden en de bruikbaarheid verbeteren. Tijdens het aanmelden bepaalt Azure AD wanneer een gebruiker moet worden geverifieerd. Azure AD kunt u intelligente beslissingen door te lezen van de gebruiker en organisatie-instellingen voor de gebruikersnaam die is opgegeven op de aanmeldingspagina. Dit is een stap naar een wachtwoord gratis toekomstige waarmee extra referenties, zoals FIDO 2.0.

## <a name="home-realm-discovery-behavior"></a>Thuisdomein detectie gedrag

Thuisdomein detectie is in het verleden beheerst door het domein dat is opgegeven bij het aanmelden of door een beleid voor thuis Realm detectie voor sommige oudere toepassingen. Bijvoorbeeld een Azure Active Directory-gebruiker in het oude detectie-gedrag, hun gebruikersnaam kan verkeerd maar nog steeds van hun organisatie referentie verzameling scherm zou aankomen. Dit gebeurt wanneer de gebruiker correct geeft de naam van de organisatie-domein 'contoso.com'. Dit gedrag is niet toegestaan voor de granulatie om aan te passen ervaringen voor een afzonderlijke gebruiker.

Voor het ondersteunen van een breder scala van referenties en de bruikbaarheid te verhogen, wordt Azure Active Directory gebruikersnaam lookup gedrag tijdens het aanmelden nu bijgewerkt. Het nieuwe gedrag kunt u intelligente beslissingen door te lezen van de tenant en gebruiker instellingen op basis van de gebruikersnaam die is opgegeven op de aanmeldingspagina. Als u wilt dit mogelijk is, wordt Azure Active Directory gecontroleerd om te zien of de gebruikersnaam die is opgegeven op de aanmeldingspagina in het opgegeven domein bestaat of de gebruiker om hun referenties te verstrekken. 

Een bijkomend voordeel van deze taak is verbeterd foutbericht messaging. Hier volgen enkele voorbeelden van de verbeterde foutberichten tijdens het aanmelden bij een toepassing die ondersteuning biedt voor Active Directory-gebruikers.

1. De gebruikersnaam is onjuist of de gebruikersnaam is nog niet zijn gesynchroniseerd met Azure AD:
  
    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/signin-realm-discovery/typo-username.png)
  
2. De domeinnaam is een typefout gemaakt:
  
    ![de domeinnaam is een typefout gemaakt of is niet gevonden](./media/signin-realm-discovery/typo-domain.png)
  
3. Gebruiker wil zich aanmelden met een bekende consumentendomein:
  
    ![aanmelden met een bekende consumentendomein](./media/signin-realm-discovery/consumer-domain.png)
  
4. Het wachtwoord is een typefout gemaakt, maar de gebruikersnaam juist is:  
  
    ![wachtwoord is een typefout gemaakt met een goede gebruikersnaam](./media/signin-realm-discovery/incorrect-password.png)
  
## <a name="additional-info"></a>Aanvullende informatie

Naast de ervaring van de gebruiker voor verbeterde aanmelden bevat deze wijziging mechanismen die misbruik van grootschalige gebruikersnaam opsomming kunnen verminderen.

Deze wijziging in eerste instantie is bedoeld voor beheerde domeinen en begint met het uitrollen van in mei 2019, maar niet start uitgebracht voor federatieve domeinen aan het einde van 2019. De exacte uitrollen datums voor federatieve domeinen, is afhankelijk van feedback van klanten.

> [!IMPORTANT]
> Deze functie wordt aanzienlijke invloed hebben op federatieve domeinen die afhankelijk zijn van het oude domeinniveau start Realm detectie om af te dwingen federation. Sommige organisaties hebben hun werknemers zich aanmelden met een gebruikersnaam die niet bestaat in Azure Active Directory, maar bevat de naam van het juiste domein, omdat de domeinnamen gebruikers momenteel omleidt naar van hun organisatie domein eindpunt getraind. Het nieuwe gedrag aanmelden niet mogelijk. De gebruiker is een melding voor het corrigeren van de gebruikersnaam en ze zich aanmelden met een gebruikersnaam die niet in Azure Active Directory bestaat zijn niet toegestaan.
>
> Als u of uw organisatie hebt de procedures die afhankelijk van het oude gedrag zijn, is het belangrijk dat beheerders van de organisatie voor het bijwerken van de documentatie bij de aanmelding en verificatie van de werknemer en met het trainen van werknemers kunnen hun Azure Active Directory-gebruikersnaam aan te melden bij gebruiken.
  
Als u problemen met het nieuwe gedrag hebt, kunt u uw opmerkingen in de **Feedback** sectie van dit artikel.  

## <a name="next-steps"></a>Volgende stappen

[Meld u in uw huisstijl aanpassen](../fundamentals/add-custom-domain.md)