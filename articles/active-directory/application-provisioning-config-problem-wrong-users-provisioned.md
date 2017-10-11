---
title: Verkeerde set gebruikers zijn ingericht op een galerie van Azure AD-toepassing | Microsoft Docs
description: Meer informatie over het om erachter te komen waarom een andere set gebruikers zijn ingericht op een andere toepassing dan u verwacht
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
ms.openlocfilehash: 85b533584c8ec15a23be32e20db7de583fced6a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Verkeerde set gebruikers zijn ingericht op een galerie van Azure AD-toepassing

Welke gebruikers zijn ingericht op de app wordt voornamelijk aangedreven door welke gebruikers en groepen zijn **toegewezen** tot de toepassing.

Gebruik de onderstaande bronnen voor meer informatie over het controleren welke gebruikers en groepen zijn toegewezen aan een toepassing in Azure Active Directory.

## <a name="assign-a-user-directly-as-an-administrator"></a>Een gebruiker toewijzen rechtstreeks als een beheerder

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo voor uw gebruiker toevoegen aan de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

Als inrichting is geconfigureerd en actief is voor een app, nieuwe gebruikers moeten worden ingericht op een toepassing in ongeveer 10 minuten. Controleer de **controlelogboeken** voor meer informatie.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Een groep rechtstreeks aan een toepassing als een beheerder toewijzen

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Schakel het selectievakje in naast de profielfoto of het logo voor uw gebruiker toevoegen aan de groep de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in om toe te voegen van deze groep de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde groepen.

Als inrichting is geconfigureerd en actief is voor een app, nieuwe gebruikers die zich in de groep moeten worden ingericht op een toepassing in ongeveer tien minuten. Controleer de **controlelogboeken** voor meer informatie.

>[!IMPORTANT]
>Het inrichten van details, naast de leden van de groepsnaam en als dit wordt ondersteund op bepaalde toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor een groepsobjecten worden weergegeven in de **inrichten** tabblad. 
>
>

Als inrichting groepen is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de 'overeenkomende ID' Lees. Dit kan de weergegeven naam of e-mailbericht alias, zoals de groep en de bijbehorende leden niet worden ingericht als de overeenkomende eigenschap leeg of niet is ingevuld voor een groep in Azure AD.

## <a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](active-directory-saas-app-provisioning.md)
