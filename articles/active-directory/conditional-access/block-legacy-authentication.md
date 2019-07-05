---
title: Het blokkeren van verouderde verificatie met Azure Active Directory (Azure AD) met voorwaardelijke toegang | Microsoft Docs
description: Leer hoe u uw beveiligingspostuur verbeteren door verouderde verificatie met behulp van Azure AD voor voorwaardelijke toegang blokkeren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60fc168a6c15e7cb8d6e4271154d99fb4136226e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509033"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Procedure: Verouderde verificatie met Azure AD met voorwaardelijke toegang blokkeren   

Als u wilt uw gebruikers eenvoudig toegang geven tot uw cloud-apps, Azure Active Directory (Azure AD) biedt ondersteuning voor tal van verificatieprotocollen, waaronder oudere authentication. Echter, verouderde protocollen bieden geen ondersteuning voor multi-factor authentication (MFA). MFA is in veel omgevingen een algemene vereiste op identiteitsdiefstal adres. 

Als uw omgeving klaar voor de verouderde verificatie blok is voor het verbeteren van de beveiliging van uw tenant, kunt u dit doel te bereiken met voorwaardelijke toegang. In dit artikel wordt uitgelegd hoe u beleid voor voorwaardelijke toegang die verouderde verificatie voor uw tenant blokkeren kunt configureren.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent: 

- De [basisconcepten](overview.md) van Azure AD voor voorwaardelijke toegang 
- De [aanbevolen procedures](best-practices.md) voor het configureren van beleid voor voorwaardelijke toegang in Azure portal

## <a name="scenario-description"></a>Scenariobeschrijving

Azure AD biedt ondersteuning voor diverse van de meest gebruikte verificatie en autorisatie protocollen, waaronder oudere authentication. Verouderde verificatie verwijst naar de protocollen die gebruikmaken van basisverificatie. Deze protocollen afdwingen niet normaal gesproken elk type tweeledige verificatie. Voorbeelden van apps die zijn gebaseerd op verouderde verificatie zijn:

- Oudere Microsoft Office-apps
- Apps met behulp van e-mailprotocollen zoals POP, IMAP en SMTP

Één-factor authentication (bijvoorbeeld gebruikersnaam en wachtwoord) is niet voldoende tegenwoordig. Wachtwoorden zijn ongeldig omdat ze gemakkelijk te raden en we (mens) ongeldige zijn bij het kiezen van goede wachtwoorden. Wachtwoorden zijn ook kwetsbaar voor tal van aanvallen zoals phishing en het wachtwoord spray. Een van de eenvoudigste dingen die u doen kunt om te beveiligen tegen bedreigingen van wachtwoord is voor het implementeren van MFA. Met MFA, zelfs als een aanvaller Hiermee haalt u in bezit is van het wachtwoord van een gebruiker, is het wachtwoord alleen niet voldoende om te verifiëren en toegang tot de gegevens.

Hoe kunt u voorkomen dat apps met behulp van verouderde verificatie van toegang tot resources van uw tenant? De aanbeveling is om te blokkeren dat ze alleen met een beleid voor voorwaardelijke toegang. Indien nodig, kunt u alleen bepaalde gebruikers en specifieke netwerklocaties apps die zijn gebaseerd op verouderde verificatie gebruiken.

Beleid voor voorwaardelijke toegang worden afgedwongen nadat de eerste factor-verificatie is voltooid. Daarom voorwaardelijke toegang is niet bedoeld als een eerste regel defense voor scenario's zoals denial-of-service (DoS) aanvallen, maar kunnen gebruikmaken van signalen van deze gebeurtenissen (bijvoorbeeld het niveau van aanmeldingsrisico, locatie van de aanvraag, enzovoort) om toegang te bepalen.

## <a name="implementation"></a>Implementatie

In deze sectie wordt uitgelegd hoe u een beleid voor voorwaardelijke toegang tot blok verouderde verificatie configureren. 

### <a name="identify-legacy-authentication-use"></a>Identificeren van verouderde verificatie gebruiken

Voordat u verouderde verificatie in uw directory blokkeren kunt, moet u eerst te begrijpen als uw gebruikers de apps die gebruikmaken van verouderde en hoe dit van invloed op uw algehele directory hebben. Azure AD aanmelden logboeken kunnen worden gebruikt om te begrijpen als u verouderde verificatie.

1. Navigeer naar de **Azure-portal** > **Azure Active Directory** > **aanmeldingen**.
1. De Client-App-kolom niet toevoegen als deze niet wordt weergegeven door te klikken op **kolommen** > **Client-App**.
1. Filteren op **Client-App** > **andere Clients** en klikt u op **toepassen**.

Filteren wordt alleen weergegeven u zich aanmeldt, probeert die zijn aangebracht door verouderde verificatieprotocollen. Te klikken op elke afzonderlijke aanmeldingspoging leert u meer informatie. De **Client-App** veld onder de **basisgegevens** tabblad wordt aangegeven welke verouderde verificatieprotocol is gebruikt.

Deze logboeken wordt aangegeven welke gebruikers zijn nog steeds afhankelijk van verouderde en welke toepassingen verouderde protocollen gebruiken om aan te verificatieaanvragen. Implementeer een beleid voor voorwaardelijke toegang voor deze gebruikers alleen voor gebruikers die niet in deze logboeken weergegeven en is bevestigd verouderde verificatie niet worden gebruikt.

### <a name="block-legacy-authentication"></a>Verouderde verificatie blokkeren 

U kunt een voorwaarde die is gekoppeld aan de client-apps die worden gebruikt voor toegang tot uw resources instellen in een beleid voor voorwaardelijke toegang. De voorwaarde van client-apps kunt u Beperk het bereik tot apps door te selecteren met behulp van verouderde **andere clients** voor **mobiele apps en bureaubladclients**.

![Andere clients](./media/block-legacy-authentication/01.png)

Voor het blokkeren van toegang voor deze apps, moet u selecteren **toegang blokkeren**.

![Toegang blokkeren](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Selecteer gebruikers en cloud-apps

Als u blokkeren van verouderde verificatie voor uw organisatie wilt, denkt u waarschijnlijk dat u dit door te selecteren doen kunt:

- Alle gebruikers
- Alle cloud-apps
- Toegang blokkeren

![Toewijzingen](./media/block-legacy-authentication/03.png)

Azure heeft een veiligheid-functie waarmee wordt voorkomen u het maken van een beleid zoals dit dat omdat deze configuratie is in strijd met de [aanbevolen procedures](best-practices.md) voor beleid voor voorwaardelijke toegang.
 
![Voor beleidsconfiguratie wordt niet ondersteund](./media/block-legacy-authentication/04.png)

De functie veiligheid is nodig omdat *blokkeren van alle gebruikers en alle cloud-apps* in potentie moet worden geblokkeerd dat de hele organisatie van de aan te melden bij uw tenant. U moet ten minste één gebruiker om te voldoen aan de minimale vereiste voor best practice uitsluiten. U kunt ook een directory-rol uitsluiten.

![Voor beleidsconfiguratie wordt niet ondersteund](./media/block-legacy-authentication/05.png)

U kunt deze functie veiligheid door één gebruiker uitsluiten van het beleid voldoen. In het ideale geval moet u enkele definiëren [beheerdersaccounts in Azure AD EMS-access](../users-groups-roles/directory-emergency-access.md) en uw beleid uitsluiten.

## <a name="policy-deployment"></a>Implementatie van beleid

Voordat u het beleid in productie plaatst, regelt:
 
- **Serviceaccounts** -gebruikersaccounts die worden gebruikt als service-accounts of door apparaten, zoals conferentie ruimte telefoons identificeren. Zorg ervoor dat deze accounts sterke wachtwoorden en ze toevoegen aan een uitgesloten groep.
- **-Aanmeldingsrapporten** : Bekijk het rapport aanmelden en zoek naar **andere client** verkeer. Bovenste gebruik identificeren en onderzoeken waarom het wordt gebruikt. Het verkeer wordt meestal gegenereerd door oudere Office-clients die geen moderne verificatie, of sommige externe e-mailapps gebruiken. Maak een plan voor gebruik van deze apps verplaatsen of als laag, is de impact op de hoogte stellen uw gebruikers dat ze meer deze apps niet gebruiken.
 
Zie voor meer informatie, [hoe moet u een nieuw beleid implementeren?](best-practices.md#how-should-you-deploy-a-new-policy).

## <a name="what-you-should-know"></a>Wat u moet weten

Blokkeren van toegang met behulp van **andere clients** ook blokkeert Exchange Online PowerShell met behulp van eenvoudige verificatie

Configureren van een beleid voor **andere clients** Hiermee blokkeert u de hele organisatie van bepaalde clients, zoals SPConnect. Dit blok treedt op omdat oudere clients worden geverifieerd op onverwachte manieren. Het probleem geldt niet voor belangrijke Office-toepassingen, zoals de oudere Office-clients.

Duurt maximaal 24 uur voor het beleid van kracht.

U kunt besturingselementen voor alle beschikbare verlenen voor de **andere clients** voorwaarde; de ervaring voor eindgebruikers is echter altijd hetzelfde - toegang geblokkeerd.

Als u verouderde verificatie met behulp van blokkeert de **andere clients** voorwaarde, u kunt ook de apparaat-platform en de locatie-voorwaarde instellen. Bijvoorbeeld, als u wilt dat alleen verouderde verificatie voor mobiele apparaten blokkeren, stel de **apparaatplatformen** voorwaarde door te selecteren:

- Android
- iOS
- Windows Phone

![Voor beleidsconfiguratie wordt niet ondersteund](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Volgende stappen

- Als u niet bekend bent met het configureren van beleid voor voorwaardelijke toegang nog, Zie [MFA vereisen voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang](app-based-mfa.md) voor een voorbeeld.
- Zie voor meer informatie over de ondersteuning van moderne verificatie [hoe moderne verificatie werkt voor Office 2013 en Office 2016 client-apps](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) 
