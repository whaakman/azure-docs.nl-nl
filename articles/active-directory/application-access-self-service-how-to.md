---
title: Het configureren van de toewijzing van selfservice toepassingen | Microsoft Docs
description: Toegang tot de toepassing Self-Service gebruikers kunnen hun eigen toepassingen zoeken inschakelen
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a>Toewijzing van selfservice toepassingen configureren

Voordat uw gebruikers toepassingen van hun Toegangsvenster automatisch detecteren kunnen, moet u inschakelen **toegang tot toepassingen Self-service** naar elke toepassing die u wilt toestaan dat gebruikers zelf detecteren en aanvragen toegang tot.

Deze functie is een uitstekende manier om tijd en geld besparen als een IT-groep, en wordt sterk aanbevolen als onderdeel van een implementatie van moderne toepassingen met Azure Active Directory.

Met deze functie kunt u het volgende doen:

-   Kunnen gebruikers zelf detecteren toepassingen van de [toepassing Toegangsvenster](https://myapps.microsoft.com/) zonder te proberen alles van de IT-groep.

-   Deze gebruikers toevoegen aan een vooraf geconfigureerde groep, zodat u kunt zien wie toegang heeft aangevraagd, toegang verwijderen en de aan hen toegewezen rollen beheren.

-   Een zakelijke goedkeurder goed te keuren toegangsaanvragen van toepassingen zodat de IT-groep niet toestaan.

-   Configureer desgewenst maximaal 10 personen die toegang tot deze toepassing kunnen goedkeuren.

-   Een bedrijf toestaan goedkeurder in te stellen de wachtwoorden die gebruikers kunt gebruiken om te aan te melden aan de toepassing, rechts van de zakelijke goedkeurder [Toegangspaneel toepassing](https://myapps.microsoft.com/).

-   Optioneel automatisch toewijzen self-service gebruikers rechtstreeks toegewezen aan de toepassingsrol van een.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Toegang tot de toepassing Self-Service gebruikers kunnen hun eigen toepassingen zoeken inschakelen

Toegang tot de toepassing zelf is een uitstekende manier om toestaan dat gebruikers zelf detecteren toepassingen, toestaan de bedrijfsgroep goedkeuren van toegang tot deze toepassingen. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan deze gebruikers voor het recht wachtwoord eenmalige aanmelding op toepassingen van hun panelen toegang toestaan.

Voor self-service toepassing toegang tot een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt inschakelen, Self-service toegang tot in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **Self-service** van navigatiemenu links aan de van de toepassing.

8.  Voor self-service toepassing toegang voor deze toepassing, schakelt u de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** in-of uitschakelen op **Ja.**

9.  Klik vervolgens op de selector naast het label om te selecteren in de groep waartoe gebruikers die aanvragen toegang tot deze toepassing moet worden toegevoegd, **voor welke groep toegewezen gebruikers worden toegevoegd?** en selecteert u een groep.

10. **Optioneel:** als u wilt een zakelijke goedkeuringsprocedure voordat gebruikers toegang hebben, stelt u de **moeten worden goedgekeurd voordat het verlenen van toegang tot deze toepassing?** in-of uitschakelen op **Ja**.

11. **Optioneel: voor toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding alleen op** als u deze bedrijven fiatteurs om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers toestaan wilt, stelt u de **fiatteurs instellen van wachtwoorden voor deze toepassing van de gebruiker toestaan?** in-of uitschakelen op **Ja**.

12. **Optioneel:** om op te geven de zakelijke fiatteurs die zijn toegestaan voor het goedkeuren van toegang tot deze toepassing, klikt u op de selector naast het label **die is toegestaan voor het goedkeuren van toegang tot deze toepassing?** maximaal 10 afzonderlijke business goedkeurders selecteren.

   >[!NOTE]
   >Groepen worden niet ondersteund.
   >
   >

13. **Optioneel:** **voor toepassingen die functies zichtbaar**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klikt u op de selector naast de **welke rol u gebruikers wilt toewijzen in deze toepassing?** om de rol waaraan u deze gebruikers worden toegewezen te selecteren.

14. Klik op de **opslaan** knop aan de bovenkant van de blade om te voltooien.

Nadat u de configuratie van Self-Service toepassing hebt voltooid, gebruikers kunnen navigeren naar hun [toepassing Toegangspaneel](https://myapps.microsoft.com/) en klik op de **+ toevoegen** om te zoeken van de apps waarmee u toegang tot selfservice hebt ingeschakeld. Zakelijke goedkeurders er ook een melding in hun [Toegangspaneel toepassing](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing die hun goedkeuring vereist aangevraagd e-mail inschakelen. 

Deze goedkeuringen ondersteuning voor één goedkeuringswerkstromen, wat betekent dat als u meerdere goedkeurders opgeeft, kan een enkele fiatteur goedkeurder toegang tot de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory instellen voor selfservicegroepsbeheer](active-directory-accessmanagement-self-service-group-management.md)
