---
title: De directory voor uw Office 365-abonnement in Azure beheren | Microsoft Docs
description: Een directory voor een Office 365-abonnement  beheren met Azure Active Directory en de klassieke Azure-portal
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 746987b7-2dfd-4b35-819d-37c1b65c5c81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: b520a5e96417fb766a757fabc384a1fc4eb0f14e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>De directory voor uw Office 365-abonnement in Azure beheren
In dit artikel wordt beschreven hoe u met behulp van de klassieke Azure-portal een directory beheert die voor een Office 365-abonnement is gemaakt. U moet de servicebeheerder of medebeheerder van een Azure-abonnement zijn om u aan te melden bij de klassieke Azure-portal. Als u nog geen Azure-abonnement hebt, kunt u zich vandaag aanmelden voor een [gratis proefperiode van 30 dagen](https://azure.microsoft.com/trial/get-started-active-directory/) en via deze koppeling binnen vijf minuten uw eerste cloudoplossing implementeren. Gebruik het werk- of schoolaccount waarmee u zich aanmeldt bij Office 365.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen.

Nadat u het Azure-abonnement hebt voltooid, kunt u zich aanmelden bij de klassieke Azure-portal en hebt u toegang tot Azure-services. Klik op de Active Directory-extensie om dezelfde directory te beheren waarmee uw Office 365-gebruikers worden geverifieerd.

Als u al een Azure-abonnement hebt, is het beheer van een extra directory ook eenvoudig. Jaap Kleefstra heeft bijvoorbeeld een Office 365-abonnement voor Contoso.com. Hij heeft ook een Azure-abonnement waarbij hij zich heeft geregistreerd met zijn Microsoft-account, msmith@hotmail.com. In dit geval beheert hij twee directory's.

| Abonnement | Office 365 | Azure |
| --- | --- | --- |
|   Weergavenaam |Contoso |De standaarddirectory voor Azure Active Directory (Azure AD) |
|   Domeinnaam |contoso.com |jkleefstrahotmail.onmicrosoft.com |

Hij wil de gebruikersidentiteiten in de Contoso-directory beheren terwijl hij is aangemeld bij Azure met zijn Microsoft-account, zodat hij Azure AD-functies zoals Multi-Factor Authentication kan inschakelen. Het proces wordt verduidelijkt in het volgende diagram.

![Diagram voor het beheren van twee onafhankelijke directory's](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

In dit geval zijn de twee directory's onafhankelijk van elkaar.

## <a name="to-manage-two-independent-directories"></a>Twee onafhankelijke directory's beheren
Jaap Kleefstra moet de volgende stappen uitvoeren om beide directory's te kunnen beheren terwijl hij is aangemeld bij Azure als msmith@hotmail.com:

> [!NOTE]
> Deze stappen kunnen alleen worden uitgevoerd wanneer een gebruiker is aangemeld met een Microsoft-account. Als de gebruiker is aangemeld met een werk- of schoolaccount, is de optie **Bestaande directory gebruiken** niet beschikbaar. Een werk- of schoolaccount kan alleen worden geverifieerd aan de hand van de basisdirectory van het account (de directory waarin het werk- of schoolaccount is opgeslagen, en die eigendom is van het bedrijf of de school).
>
>

1. Meld u bij de [klassieke Azure-portal](https://manage.windowsazure.com) aan als msmith@hotmail.com.
2. Klik achtereenvolgens op **Nieuw** > **App Services** > **Active Directory** > **Directory** > **Aangepast maken**.
3. Klik op Bestaande directory gebruiken en schakel het selectievakje **Ik kan nu worden afgemeld** in.
4. Meld u aan bij de klassieke Azure-portal als globale beheerder van Contoso.onmicrosoft.com (bijvoorbeeld msmith@contoso.com).
5. Wanneer u wordt gevraagd **De Contoso-directory gebruiken met Azure?**, klikt u op **Doorgaan**.
6. Klik op **Nu afmelden**.
7. Meld u bij de klassieke Azure-portal aan als msmith@hotmail.com. De Contoso-directory en de Standaarddirectory worden weergegeven in de Active Directory-extensie.

Wanneer u deze stappen hebt uitgevoerd, is msmith@hotmail.com een globale beheerder in de Contoso-directory.

## <a name="to-administer-resources-as-the-global-admin"></a>Resources beheren als globale beheerder
Stel nu dat Dena Vloet websites en databaseresources moet beheren die zijn gekoppeld aan het Azure-abonnement van msmith@hotmail.com. Voordat ze dit kan doen, moet Jaap Kleefstra deze extra stappen uitvoeren:

1. Zich aanmelden bij de [klassieke Azure-portal](https://manage.windowsazure.com) met het servicebeheerdersaccount voor het Azure-abonnement (in dit voorbeeld msmith@hotmail.com).
2. Het abonnement overbrengen naar de Contoso-directory: klik achtereenvolgens op **Instellingen** > **Abonnementen** > selecteer het abonnement > **Directory bewerken** > selecteer **Contoso (Contoso.com)**. Als onderdeel van de overdracht worden werk- of schoolaccounts die medebeheerder zijn van het abonnement, verwijderd.
3. Dena Vloet toevoegen als medebeheerder van het abonnement: klik achtereenvolgens op **Instellingen** > **Beheerders** > selecteer het abonnement > **Toevoegen** > typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Volgende stappen
Zie [De manier waarop een abonnement is gekoppeld aan een directory](active-directory-how-subscriptions-associated-directory.md) voor meer informatie over de relatie tussen abonnementen en directory's.
