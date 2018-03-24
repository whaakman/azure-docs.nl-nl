---
title: 'Azure Active Directory B2C: Selfservice voor wachtwoordherstel | Microsoft Docs'
description: Een onderwerp aan te tonen het instellen van de selfservice voor wachtwoordherstel voor uw consumenten in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: f38473989f90bfe6d35bffb17a02a892ad08cf5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Selfservice voor wachtwoordherstel voor uw consumenten instellen
Uw consumenten (die zich hebben geregistreerd voor lokale accounts) kunnen hun wachtwoorden op hun eigen opnieuw instellen met de functie zelf uw wachtwoord opnieuw instellen. Dit vermindert de werkbelasting van uw medewerkers, met name als uw toepassing heeft miljoenen consumenten regelmatig gebruikt. Op dit moment wordt alleen ondersteund als een methode voor het herstellen met behulp van een geverifieerde e-mailadres. Er wordt aanvullende herstelpunten methoden (geverifieerde telefoonnummer, beveiligingsvragen, enz.) in de toekomst toegevoegd.

> [!NOTE]
> In dit artikel is van toepassing op zelf uw wachtwoord opnieuw instellen die worden gebruikt in de context van een beleid voor aanmelden. Als u volledig aanpasbare wachtwoord opnieuw instellen van beleidsregels aangeroepen vanuit uw app nodig hebt, raadpleegt u [in dit artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Standaard uw directory geen zelf uw wachtwoord opnieuw instellen is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als Abonnementsbeheerder. Dit is hetzelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt voor het maken van uw directory.
2. Open Active Directory (in de navigatiebalk aan de linkerkant).
3. Selecteer **eigenschappen**.
4. Schuif omlaag naar de **selfservice voor wachtwoordherstel ingeschakeld** sectie- en uitschakelen van deze **alle**. 
5. Klik op **opslaan** boven aan de pagina. U bent nu klaar!

Als u wilt testen, gebruikt u de functie 'Nu uitvoeren' op elk beleid aanmelden met lokale accounts als een id-provider. Op de lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **geen toegang tot uw account?** om te controleren of de consumer-ervaring.

> [!NOTE]
> De zelf uw wachtwoord opnieuw instellen van pagina's kunnen worden aangepast via de [functie huisstijl](../active-directory/customize-branding.md).
> 
> 

