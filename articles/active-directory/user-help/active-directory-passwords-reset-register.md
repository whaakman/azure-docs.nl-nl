---
title: Registreren voor self-service voor wachtwoord opnieuw instellen - Azure Active Directory | Microsoft Docs
description: Registreert gegevens van de verificatie voor Azure AD Self-service voor wachtwoord opnieuw instellen
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2018
ms.author: lizross
ms.openlocfilehash: c7291af37e0691c22d93ac84e66fabba91890726
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55186804"
---
# <a name="register-for-self-service-password-reset"></a>Registreren voor de selfservice voor het opnieuw instellen van een wachtwoord

> [!IMPORTANT]
> Bent u hier terechtgekomen omdat u niet aanmelden? Als dit het geval is, raadpleegt u [uw werk- of schoolaccount wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

U kunt uw wachtwoord opnieuw instellen of uw account ontgrendelen door uzelf als u Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR) als een eindgebruiker. Voordat u deze functie gebruiken kunt, moet u uw verificatiemethoden registreren of bevestigen van de vooraf gedefinieerde verificatiemethoden die uw beheerder heeft ingevuld.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Verificatiegegevens registreren of bevestigen met SSPR

1. Open de webbrowser op uw apparaat en Ga naar de [pagina voor de registratie voor wachtwoordherstel](https://aka.ms/ssprsetup).
2. Voer uw gebruikersnaam en het wachtwoord die uw beheerder heeft.
3. Afhankelijk van hoe uw IT-personeel dingen heeft geconfigureerd, zijn een of meer van de volgende opties beschikbaar voor u configureren en controleren. Als uw beheerder uw toestemming om uw informatie te gebruiken heeft, kunnen ze een deel van de gegevens voor u vullen.
    * **Telefoon (werk)**: Alleen de beheerder kan deze optie instellen.
    * **Telefoon voor authenticatie**: Deze optie instelt op een ander telefoonnummer op dat u toegang tot hebt. Een voorbeeld is een mobiele telefoon die een SMS-bericht of een aanroep kan ontvangen.
    * **Verificatie-e-mailbericht**: Deze optie instelt op een alternatief e-mailadres waartoe u toegang zonder het wachtwoord die u wilt instellen.
    * **Vragen over de beveiliging**: Uw beheerder heeft goedgekeurd dat deze lijst met vragen u moet beantwoorden. U kan niet dezelfde vraag gebruiken of meer dan één keer te beantwoorden.
4. Geef en controleer de informatie die uw beheerder vereist. Als meer dan één optie beschikbaar is, het is raadzaam dat u verschillende methoden registreert. Dit biedt u flexibiliteit wanneer u een van de methoden is niet beschikbaar. Een voorbeeld is wanneer u onderweg bent en u geen toegang tot uw zakelijke telefoon.

    ![Registreer verificatiemethoden en selecteer voltooien][Register]

5. Selecteer **voltooien**. U kunt nu SSPR gebruiken als u in de toekomst wilt.

Als u gegevens voor **telefoon voor authenticatie** of **Verificatieadres**, het is niet zichtbaar in de algemene map. De enige personen die deze gegevens kunnen zien, zijn u en uw beheerder. U kunt alleen de antwoorden op uw beveiligingsvragen zien.

Uw beheerders kunnen vereisen dat u kunt bevestigen dat uw verificatiemethoden na een bepaalde periode om ervoor te zorgen dat u hebt nog steeds de juiste methoden die zijn geregistreerd.

## <a name="common-problems-and-their-solutions"></a>Veelvoorkomende problemen en oplossingen

 Hier volgen enkele algemene foutgevallen en hun oplossingen:

| Foutaanvraag| Welk foutbericht ziet u?| Oplossing |
| --- | --- | --- |
| Er verschijnt een pagina 'Neem contact op met uw beheerder' na het invoeren van mijn gebruikers-ID | Neem contact op met de beheerder. <br> <br> Er is vastgesteld dat uw wachtwoord niet wordt beheerd door Microsoft. We zijn als gevolg hiervan kan niet automatisch uw wachtwoord opnieuw instellen. <br> <br> Neem contact op met uw IT-personeel voor verdere ondersteuning. | U ziet dit bericht omdat uw IT-afdeling uw wachtwoord in uw on-premises-omgeving beheert en kunt u uw wachtwoord opnieuw instellen niet de **geen toegang tot uw account** koppeling. <br> <br> Als u wilt uw wachtwoord opnieuw instellen, contact op met uw IT-personeel rechtstreeks voor hulp. Laat weten dat u wilt uw wachtwoord opnieuw instellen, zodat ze deze functie voor u inschakelen kunnen.|
| Er verschijnt een foutbericht 'uw account is niet ingeschakeld voor wachtwoord opnieuw instellen' na het invoeren van mijn gebruikers-ID | Uw account is niet ingeschakeld voor wachtwoord opnieuw instellen. <br> <br> Onze excuses, maar uw IT-afdeling heeft niet instellen van uw account voor gebruik met deze service. <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat uw IT-personeel is niet ingeschakeld voor wachtwoordherstel in voor uw organisatie van de **geen toegang tot uw account** koppelen of u voor het gebruik van de functie nog niet is gelicentieerd. <br> <br> Als u wilt uw wachtwoord opnieuw instellen, selecteert u de **contact op met een beheerder** koppeling. Een e-mailbericht ontvangt van uw bedrijf IT-personeel. Het e-mailbericht hen te laten weten dat u wilt dat uw wachtwoord opnieuw instellen, zodat ze deze functie voor u inschakelen kunnen. |
| Er verschijnt een foutbericht ' we kunnen uw account niet verifiëren' na het invoeren van mijn gebruikers-ID | We kunnen uw account niet verifiëren. <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat u hebt ingeschakeld voor wachtwoord opnieuw instellen, maar u dit nog niet hebt geregistreerd voor het gebruik van de service. Als u wilt registreren voor wachtwoord opnieuw instellen, gaat u naar de [pagina voor de registratie voor wachtwoordherstel](https://aka.ms/ssprsetup) nadat u hebt toegang heeft gekregen bij uw account. <br> <br> Als u wilt uw wachtwoord opnieuw instellen, selecteert u de **contact op met een beheerder** koppelen aan een e-mailbericht verzenden van uw bedrijf IT-personeel. |

## <a name="next-steps"></a>Volgende stappen

* [Uw wachtwoord wijzigen via Self-service voor wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md)
* [Registratiepagina voor het opnieuw instellen van een wachtwoord](https://aka.ms/ssprsetup)
* [Portal voor het opnieuw instellen van een wachtwoord](https://passwordreset.microsoftonline.com/)
* [Wanneer u zich niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Pagina voor de registratie opnieuw instellen van wachtwoord met geregistreerde methoden en de knop Voltooien"

