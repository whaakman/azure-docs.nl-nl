---
title: 'Azure Active Directory B2C: Selfservice voor wachtwoordherstel | Microsoft Docs'
description: Een onderwerp aan te tonen het instellen van de selfservice voor wachtwoordherstel voor uw consumenten in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Selfservice voor wachtwoordherstel voor uw consumenten instellen
Uw consumenten (die zich hebben geregistreerd voor lokale accounts) kunnen hun wachtwoorden op hun eigen opnieuw instellen met de functie zelf uw wachtwoord opnieuw instellen. Dit vermindert de werkbelasting van uw medewerkers, met name als uw toepassing heeft miljoenen consumenten regelmatig gebruikt. Op dit moment wordt alleen ondersteund als een methode voor het herstellen met behulp van een geverifieerde e-mailadres. Er wordt aanvullende herstelpunten methoden (geverifieerde telefoonnummer, beveiligingsvragen, enz.) in de toekomst toegevoegd.

> [!NOTE]
> In dit artikel is van toepassing op zelf uw wachtwoord opnieuw instellen die worden gebruikt in de context van een beleid voor aanmelden. Als u volledig aanpasbare wachtwoord opnieuw instellen van beleidsregels aangeroepen vanuit uw app nodig hebt, raadpleegt u [in dit artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Standaard uw directory geen zelf uw wachtwoord opnieuw instellen is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com/) als abonnementsbeheerder. Dit is hetzelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt voor het maken van uw directory.
2. Navigeer naar de Active Directory-extensie in de navigatiebalk aan de linkerkant.
3. Zoeken naar uw map onder de **Directory** tabblad en klik erop.
4. Klik op het tabblad **Configureren**.
5. Schuif omlaag naar de **beleid opnieuw instellen van wachtwoorden** sectie en in-/ uitschakelen de **gebruikers die zijn ingeschakeld voor wachtwoordherstel** optie naar **Ja**. U ziet dat de **alternatief e-mailadres** optie is ingeschakeld; ongewijzigd laten.
   
    ![Self-service voor wachtwoord opnieuw instellen](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Klik op **Opslaan** onder aan de pagina. U bent nu klaar!

Als u wilt testen, gebruikt u de functie 'Nu uitvoeren' op elk beleid aanmelden met lokale accounts als een id-provider. Op de lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **geen toegang tot uw account?** om te controleren of de consumer-ervaring.

> [!NOTE]
> De zelf uw wachtwoord opnieuw instellen van pagina's kunnen worden aangepast via de [functie huisstijl](../active-directory/active-directory-add-company-branding.md).
> 
> 

