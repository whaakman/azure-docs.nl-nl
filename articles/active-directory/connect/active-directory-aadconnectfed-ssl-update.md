---
title: 'Azure AD Connect: Bijwerken van het SSL-certificaat voor een farm met Active Directory Federation Services (AD FS) | Microsoft Docs'
description: De Documentdetails van dit van de stappen voor het bijwerken van het SSL-certificaat van een AD FS-farm met behulp van Azure AD Connect.
services: active-directory
keywords: Azure ad connect, AD FS ssl-update, adfs-certificaat updates, AD FS-certificaat wijzigen, nieuwe AD FS-certificaat, AD FS-certificaat, update AD FS ssl-certificaat en update ssl-certificaat adfs AD FS ssl-certificaat, AD FS, ssl, certificaat, AD FS-service configureren certificaat voor serviceberichten, update-federation, federatie configureren, aad connect
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: 87807a203d71b3abfe3e93132eb7d0b82b14b4ee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Bijwerken van het SSL-certificaat voor een farm met Active Directory Federation Services (AD FS)

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u Azure AD Connect kunt gebruiken voor het bijwerken van het SSL-certificaat voor een farm met Active Directory Federation Services (AD FS). Het hulpprogramma Azure AD Connect kunt u eenvoudig de SSL-certificaat voor de AD FS-farm bijwerken, zelfs als de gebruiker aanmelden methode geselecteerd niet AD FS is.

U kunt de hele bewerking van het SSL-certificaat voor de AD FS-farm bijwerken voor alle federatieve en -servers voor Webtoepassingsproxy (WAP) in drie eenvoudige stappen uitvoeren:

![Drie stappen](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>Zie voor meer informatie over certificaten die worden gebruikt door AD FS, [Understanding-certificaten die worden gebruikt door AD FS](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Vereisten

* **AD FS-Farm**: Zorg ervoor dat uw AD FS-farm op basis van Windows Server 2012 R2 of hoger.
* **Azure AD Connect**: Zorg ervoor dat de versie van Azure AD Connect 1.1.443.0 of hoger. Gebruikt u de taak **Update AD FS SSL-certificaat**.

![SSL-taak bijwerken](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Stap 1: Geef informatie op AD FS-farm

Azure AD Connect probeert te verkrijgen van informatie over de AD FS-farm automatisch door:
1. Opvragen van de Farmgegevens van AD FS (WindowsServer 2016 of hoger).
2. Verwijst naar de informatie uit de vorige wordt uitgevoerd, die worden lokaal opgeslagen met Azure AD Connect.

U kunt de lijst met servers die worden weergegeven door het toevoegen of verwijderen van de servers om de huidige configuratie van de AD FS-farm weer te wijzigen. Zodra de server-informatie is opgegeven, geeft Azure AD Connect de connectiviteit en de huidige status van de SSL-certificaat.

![AD FS-serverinformatie](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Als de lijst een server die niet langer deel uitmaakt van de AD FS-farm bevat, klikt u op **verwijderen** server verwijderen uit de lijst met servers in uw AD FS-farm.

![Offline-server in de lijst](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Een server verwijderen uit de lijst met servers voor een AD FS-farm in Azure AD Connect is een lokale bewerking en de gegevens voor de AD FS-farm die lokaal wordt onderhouden door Azure AD Connect-updates. Azure AD Connect niet de configuratie op de AD FS in overeenstemming met de wijziging niet wijzigen.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>Stap 2: Geef een nieuw SSL-certificaat

Nadat u de informatie over AD FS-farm-servers hebt bevestigd, wordt u gevraagd Azure AD Connect voor het nieuwe SSL-certificaat. Geef een PFX-certificaat wachtwoord om door te gaan met de installatie.

![SSL-certificaat](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

Nadat u het certificaat opgeeft, wordt Azure AD Connect gaat door een reeks vereisten. Controleer of het certificaat om ervoor te zorgen dat het certificaat onjuist voor de AD FS-farm is:

-   De onderwerpnaam naam/alternatieve onderwerpnaam voor het certificaat is ofwel hetzelfde als de naam van de federation-service of een jokertekencertificaat is.
-   Het certificaat is geldig voor meer dan 30 dagen.
-   De vertrouwensketen van het certificaat is geldig.
-   Het certificaat is beveiligd met een wachtwoord.

## <a name="step-3-select-servers-for-the-update"></a>Stap 3: Servers voor de update selecteren

Selecteer de servers waarvoor Beveiligingsaanmelding moet het SSL-certificaat dat is bijgewerkt in de volgende stap. Servers die offline zijn, kunnen niet worden geselecteerd voor de update.

![Selecteer servers bijwerken](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

Na het voltooien van de configuratie wordt het bericht geeft de status van de update en bevat een optie om te controleren of de AD FS-aanmeldingspagina weergegeven in Azure AD Connect.

![Configuratie voltooien](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Veelgestelde vragen

* **Wat moet de onderwerpnaam van het certificaat voor de nieuwe AD FS SSL-certificaat?**

    Azure AD Connect wordt gecontroleerd of de naam/alternatieve onderwerpnaam onderwerpnaam van het certificaat de naam van de federation-service bevat. Bijvoorbeeld, als de naam van uw federation service fs.contoso.com is, moet de onderwerpnaam naam/alternatieve onderwerpnaam fs.contoso.com.  Jokertekens-certificaten worden ook geaccepteerd.

* **Waarom moet ik voor referenties opnieuw op de pagina WAP-server?**

    Als de referenties die u opgeeft voor de verbinding met AD FS-servers niet ook de bevoegdheid voor het beheren van de WAP-servers hebt, vervolgens Azure AD Connect wordt gevraagd om referenties die u Administrator-bevoegdheden op de WAP-servers hebt.

* **De server wordt weergegeven als offline. Wat moet ik doen?**

    Azure AD Connect kan een bewerking niet uitvoeren als de server offline is. Als de server deel van de AD FS-farm uitmaakt, controleert u de verbinding met de server. Nadat u het probleem hebt opgelost, drukt u op het pictogram Vernieuwen om de status in de wizard te werken. Als de server is onderdeel van de farm eerder, maar nu niet meer bestaat, klikt u op **verwijderen** onderhoudt die Azure AD Connect verwijderen uit de lijst met servers. De configuratie van de AD FS niet van invloed op de server verwijderen uit de lijst in Azure AD Connect. De taak wordt uitgevoerd als u gebruikmaakt van AD FS in Windows Server 2016 of hoger, de server blijft in de configuratie-instellingen en opnieuw in de volgende keer wordt weergegeven.

* **Kan ik een subset van mijn webfarmservers bijwerken met de nieuwe SSL-certificaat**

    Ja. U kunt altijd de taak uitvoeren **SSL-certificaat bijwerken** opnieuw naar de resterende servers bijwerken. Op de **geselecteerde servers voor het SSL-certificaat updates** pagina kunt u de lijst met servers sorteren op **SSL vervaldatum** eenvoudig toegang tot de servers die nog niet zijn bijgewerkt.

* **Ik heb de server in de vorige uitvoering verwijderd, maar deze nog steeds wordt weergegeven als offline en weergegeven op de pagina met AD FS-Servers. Waarom is de offline-server nog steeds er zelfs nadat ik verwijderd?**

    De server verwijderen uit de lijst in Azure AD Connect verwijderen niet in de AD FS-configuratie. Azure AD Connect verwijst naar AD FS (WindowsServer 2016 of hoger) voor informatie over de farm. Als de server nog steeds aanwezig zijn in de AD FS-configuratie, wordt het weergegeven in de lijst.  

## <a name="next-steps"></a>Volgende stappen

- [Azure AD Connect en federatie](active-directory-aadconnectfed-whatis.md)
- [Active Directory Federation Services-beheer en aanpassingen met Azure AD Connect](active-directory-aadconnect-federation-management.md)
