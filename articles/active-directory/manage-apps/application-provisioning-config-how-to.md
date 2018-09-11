---
title: Het inrichten van gebruikers tot een toepassing met Azure AD-galerie configureren | Microsoft Docs
description: Hoe kunt u snel uitgebreide gebruikersaccount inrichten en -opheffing in toepassingen aanwezig zijn in de Azure AD-Toepassingsgalerie configureren
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: ed2ace333b5bdf5210a1681f39dc60248117074e
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356810"
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a>Het configureren van inrichten van gebruikers naar een toepassing in Azure AD-galerie

*Het inrichten van gebruikersaccounts* is de handeling van het maken, bijwerken en/of gebruikersaccountrecords in lokale gebruikersarchief profiel van een toepassing. De meeste SaaS-toepassingen en cloud slaat de gebruikers, rollen en machtigingen in hun eigen lokale gebruiker profiel store en aanwezigheid van dergelijke een gebruikersrecord in hun lokale archief is *vereist* voor eenmalige aanmelding en samen te werken.

In de Azure-portal, de **Provisioning** tabblad in het navigatiedeelvenster links in voor een Enterprise-App wordt weergegeven welke inrichting modi voor die app worden ondersteund. Dit kan een van twee waarden zijn:

## <a name="configuring-an-application-for-manual-provisioning"></a>Configureren van een toepassing voor het inrichten van de handleiding

*Handmatige* inrichting betekent dat gebruikersaccounts handmatig met behulp van de methoden die door deze app moeten worden gemaakt. Dit kan betekenen dat zich aanmeldt bij de beheerdersportal voor die app en het toevoegen van gebruikers met behulp van een web gebaseerde gebruikersinterface. Of het een werkblad met details van gebruiker-account, met behulp van een mechanisme dat is opgegeven door de toepassing kan uploaden. Raadpleeg de documentatie van de app of neem contact op met de app-ontwikkelaar om te bepalen wat mechanismen zijn beschikbaar.

Als handmatig de enige modus die voor een bepaalde toepassing worden weergegeven is, betekent dit dat er geen automatische Azure AD connector inrichting is gemaakt voor de app nog. Of betekent dit dat de app biedt geen ondersteuning voor de API voor het beheer van vereiste gebruiker waarop een automatische inrichting connector bouwen.

Als u wilt het aanvragen van ondersteuning voor automatische inrichting voor een bepaalde app, vul u kunt een aanvraag met de [Azure Active Directory-toepassingsaanvragen](https://aka.ms/aadapprequest).

## <a name="configuring-an-application-for-automatic-provisioning"></a>Een toepassing configureren voor automatische inrichting

*Automatische* betekent dat een Azure AD connector wordt ingericht voor deze toepassing is ontwikkeld. Zie voor meer informatie over de Azure AD provisioning-service en hoe het werkt, [automatiseren van Gebruikersinrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Zie voor meer informatie over het inrichten van specifieke gebruikers en groepen met een toepassing [beheren van het inrichten van gebruikersaccounts voor bedrijfs-apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

De werkelijke vereiste stappen voor het inschakelen en configureren van automatische inrichting variëren afhankelijk van de toepassing.

>[!NOTE]
>U moet beginnen met zoeken naar de setup-zelfstudie die specifiek zijn voor het instellen van de inrichting voor uw toepassing, en na deze stappen voor het configureren van de app en de Azure AD de inrichting verbinding te maken. 
>
>

Zelfstudies voor App kunnen worden gevonden op [lijst met zelfstudies over het SaaS-Apps integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Een belangrijke dingen die u moet overwegen bij het instellen van de inrichting worden om te controleren en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD aan de toepassing definiëren. Dit omvat instellen van de 'overeenkomende eigenschap"die worden gebruikt voor het uniek identificeren en overeenkomen met de gebruikers/groepen tussen de twee systemen. Voor meer informatie over dit belangrijk proces.

## <a name="next-steps"></a>Volgende stappen
[Kenmerktoewijzingen voor Gebruikersinrichting voor SaaS-toepassingen in Azure Active Directory aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

