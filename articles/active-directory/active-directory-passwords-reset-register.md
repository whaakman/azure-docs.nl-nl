---
title: Registreren voor selfservice voor wachtwoordherstel - Azure Active Directory
description: Registreren verificatiegegevens voor self-service Azure AD-wachtwoord opnieuw instellen
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: barlan
ms.custom: end-user;seohack1
ms.openlocfilehash: ffdffc0c471970e5a7e7a0bb291658cefe99ed71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="register-for-self-service-password-reset"></a>Registreren voor de selfservice voor het opnieuw instellen van een wachtwoord

> [!IMPORTANT]
> Weet u hier omdat u niet kunt aanmelden? Zo ja, Zie [uw werk of school-wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

U kunt uw wachtwoord opnieuw instellen of uw account door uzelf te ontgrendelen als u Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR) gebruikt als een eindgebruiker. Voordat u deze functionaliteit gebruiken kunt, die u moet uw verificatiemethoden registreren of bevestigt de vooraf gedefinieerde verificatiemethoden die de beheerder heeft ingevuld.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Verificatiegegevens registreren of bevestigen met SSPR

1. Open de webbrowser op het apparaat en Ga naar de [wachtwoordherstel registratiepagina](http://aka.ms/ssprsetup).
2. Voer uw gebruikersnaam en het wachtwoord dat uw beheerder heeft.
3. Afhankelijk van hoe uw IT-personeel dingen heeft geconfigureerd, zijn een of meer van de volgende opties zijn beschikbaar om te configureren en te verifiëren. Als de beheerder uw toestemming uw informatie te gebruiken heeft, kunnen ze de enkele van de gegevens voor u invullen.
    * **Telefoon (werk)**: alleen de beheerder deze optie kunt instellen.
    * **Telefoon voor authenticatie**: deze optie instelt op een ander telefoonnummer op dat u toegang tot hebt. Een voorbeeld is een mobiele telefoon die een tekst of een aanroep kan ontvangen.
    * **Verificatie e**: deze optie instelt op een alternatieve e-mailadres dat u toegang hebt tot zonder het wachtwoord dat u wilt instellen.
    * **Beveiligingsvragen**: deze lijst met vragen te beantwoorden is goedgekeurd door uw beheerder. Gebruik deze vraag of is niet meer dan één keer te beantwoorden.
4. Geef en controleer de informatie die de beheerder vereist. Als meer dan één optie beschikbaar is, het is raadzaam dat u meerdere methoden registreren. Dit biedt flexibiliteit bij een van de methoden is niet beschikbaar. Een voorbeeld is wanneer u onderweg bent en u geen toegang tot uw zakelijke telefoon bent.

    ![Verificatiemethoden registreren en selecteer voltooien][Register]

5. Selecteer **voltooien**. U kunt nu SSPR gebruiken wanneer u in de toekomst moet.

Als u gegevens voor invoert **telefoon voor authenticatie** of **verificatie e**, is niet zichtbaar in de globale adreslijst. De enige personen die deze gegevens kunnen zien, zijn u en uw beheerder. U kunt alleen de antwoorden op beveiligingsvragen zien.

Uw beheerders moet u mogelijk uw verificatiemethoden na een periode om ervoor te zorgen dat u de juiste methoden geregistreerd nog bevestigen.

## <a name="common-problems-and-their-solutions"></a>Veelvoorkomende problemen en oplossingen

 Hier volgen enkele veelvoorkomende foutgevallen en hun oplossingen:

| Foutaanvraag| Welke fout ziet u?| Oplossing |
| --- | --- | --- |
| Een pagina 'Neem contact op met uw beheerder' verschijnt na het invoeren van mijn gebruikers-ID | Neem contact op met de beheerder. <br> <br> We hebben vastgesteld dat het wachtwoord voor uw gebruikersaccount niet wordt beheerd door Microsoft. Als gevolg hiervan zijn er niet automatisch opnieuw instellen van uw wachtwoord. <br> <br> Neem contact op met uw IT-personeel voor verdere assistentie. | U ziet dit bericht omdat uw IT-personeel beheert uw wachtwoord in uw on-premises omgeving en kunt u uw wachtwoord opnieuw instellen niet de **geen toegang tot uw account** koppeling. <br> <br> Om uw wachtwoord opnieuw instellen, moet u contact op met uw IT-medewerkers rechtstreeks voor hulp. Laat ze weten dat u wilt uw wachtwoord opnieuw instellen zodat ze voor u deze functie kunnen inschakelen.|
| Wordt een foutbericht 'uw account is niet ingeschakeld voor wachtwoordherstel' na het invoeren van mijn gebruikers-ID | Uw account is niet ingeschakeld voor wachtwoordherstel. <br> <br> We vinden het jammer, maar uw IT-personeel niet uw account voor gebruik met deze service is ingesteld. <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat uw IT-personeel niet ingeschakeld voor wachtwoordherstel voor uw organisatie van de **geen toegang tot uw account** koppelen of u de functie is niet een licentie. <br> <br> Als u wilt uw wachtwoord opnieuw instelt, selecteert u de **contact op met een beheerder** koppeling. Een e-mailbericht wordt verzonden van uw bedrijf IT-personeel. Het e-mailbericht hen te laten weten dat u wilt uw wachtwoord opnieuw instellen zodat ze voor u deze functie kunnen inschakelen. |
| Wordt een foutbericht ' we kunnen uw account niet verifiëren' na het invoeren van mijn gebruikers-ID | We kunnen uw account niet verifiëren. <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat u bent ingeschakeld voor wachtwoordherstel, maar u dit nog niet hebt geregistreerd voor het gebruik van de service. Als u wilt registreren voor wachtwoord opnieuw instellen, gaat u naar de [wachtwoordherstel registratiepagina](http://aka.ms/ssprsetup) nadat u toegang tot je account hebt hersteld. <br> <br> Als u wilt uw wachtwoord opnieuw instelt, selecteert u de **contact op met een beheerder** koppelen aan een e-mailbericht verzenden naar uw bedrijf IT-personeel. |

## <a name="next-steps"></a>Volgende stappen

* [Uw wachtwoord wijzigen met behulp van de selfservice voor wachtwoordherstel](active-directory-passwords-update-your-own-password.md)
* [Registratiepagina voor het opnieuw instellen van een wachtwoord](http://aka.ms/ssprsetup)
* [Portal voor het opnieuw instellen van een wachtwoord](https://passwordreset.microsoftonline.com/)
* [Wanneer u kan niet aanmelden bij je Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Wachtwoord opnieuw instellen registratiepagina weergegeven met de geregistreerde methoden en op de knop Voltooien"

