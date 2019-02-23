---
title: Azure AD Connect - Update het SSL-certificaat voor AD FS-farm | Microsoft Docs
description: De Documentdetails van dit van de stappen voor het bijwerken van het SSL-certificaat van AD FS-farm met behulp van Azure AD Connect.
services: active-directory
manager: daveba
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory  
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/09/2018
ms.subservice: hybrid
author: billmath
ms.custom: seohack1
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39ac0e9cf11a0c6c212c4beadb6635ad2b6b056d
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734707"
---
# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>De SSL-certificaat voor een Active Directory Federation Services (AD FS)-farm bijwerken

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u Azure AD Connect kunt gebruiken om bij te werken van het SSL-certificaat voor een farm met Active Directory Federation Services (AD FS). Het hulpprogramma Azure AD Connect kunt u eenvoudig de SSL-certificaat voor de AD FS-farm bijwerken, zelfs als de gebruiker aanmelden methode geselecteerd geen AD FS is.

U kunt de hele bewerking van het SSL-certificaat voor de AD FS-farm bijwerken met alle federation en servers van Webtoepassingsproxy (WAP) in drie eenvoudige stappen uitvoeren:

![Drie stappen](./media/how-to-connect-fed-ssl-update/threesteps.png)


>[!NOTE]
>Zie voor meer informatie over certificaten die worden gebruikt door AD FS, [wat zijn certificaten die worden gebruikt door AD FS](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Vereisten

* **AD FS-Farm**: Zorg ervoor dat uw AD FS-farm op basis van Windows Server 2012 R2 of hoger.
* **Azure AD Connect**: Zorg ervoor dat de versie van Azure AD Connect 1.1.553.0 of hoger. Gebruikt u de taak **Update AD FS SSL-certificaat**.

![SSL-taak bijwerken](./media/how-to-connect-fed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Stap 1: AD FS-farm informatie opgeven

Azure AD Connect probeert om informatie te verkrijgen over de AD FS-farm automatisch door:
1. Het uitvoeren van query's is de informatie van de farm van AD FS (WindowsServer 2016 of hoger).
2. Verwijst naar de gegevens uit vorige sessies, die worden lokaal opgeslagen met Azure AD Connect.

U kunt de lijst met servers die worden weergegeven door het toevoegen of verwijderen van de servers om de huidige configuratie van de AD FS-farm weer te geven. Zodra de server-informatie is opgegeven, wordt Azure AD Connect de connectiviteit en de huidige status van de SSL-certificaat.

![Gegevens van uw AD FS](./media/how-to-connect-fed-ssl-update/adfsserverinfo.png)

Als de lijst een server die niet langer deel uit van de AD FS-farm bevat, klikt u op **verwijderen** de server verwijderen uit de lijst met servers in uw AD FS-farm.

![Offline-server in de lijst](./media/how-to-connect-fed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Een server verwijderen uit de lijst met servers voor een AD FS-farm in Azure AD Connect is een lokale bewerking en de gegevens voor de AD FS-farm die lokaal wordt onderhouden door Azure AD Connect-updates. Azure AD Connect heeft niet de configuratie op de AD FS in overeenstemming met de wijziging niet wijzigen.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>Stap 2: Geef een nieuwe SSL-certificaat

Nadat u hebt vastgesteld dat de informatie over AD FS-farm-servers, vraagt Azure AD Connect voor het nieuwe SSL-certificaat. Geef een PFX-certificaat beveiligd met een wachtwoord om door te gaan met de installatie.

![SSL-certificaat](./media/how-to-connect-fed-ssl-update/certificate.png)

Nadat u het certificaat hebt opgegeven, wordt Azure AD Connect gaat via een reeks vereisten. Controleer of het certificaat om ervoor te zorgen dat het certificaat onjuist voor de AD FS-farm is:

-   De onderwerpnaam van de naam/alternatieve onderwerpnaam voor het certificaat is dezelfde als de naam van de federation-service of het is een certificaat met jokertekens.
-   Het certificaat is geldig gedurende meer dan 30 dagen.
-   De vertrouwensketen van het certificaat is geldig.
-   Het certificaat is beveiligd met een wachtwoord.

## <a name="step-3-select-servers-for-the-update"></a>Stap 3: Selecteer voor de update-servers

Selecteer de servers die u moeten het SSL-certificaat dat is bijgewerkt in de volgende stap. Servers die offline zijn, kunnen niet worden geselecteerd voor de update.

![Selecteer servers om bij te werken](./media/how-to-connect-fed-ssl-update/selectservers.png)

Nadat u de configuratie hebt voltooid, wordt Azure AD Connect het bericht weergegeven dat geeft de status van de update en biedt een optie om te controleren of de AD FS-aanmelding.

![Configuratie voltooid](./media/how-to-connect-fed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Veelgestelde vragen

* **Wat moet de onderwerpnaam van het certificaat voor de nieuwe AD FS SSL-certificaat?**

    Azure AD Connect controleert of de naam/alternatieve onderwerpnaam onderwerpnaam van het certificaat de naam van de federation-service bevat. Bijvoorbeeld, als de naam van uw federation service fs.contoso.com is, moet de naam voor onderwerp de naam/alternatieve onderwerpnaam fs.contoso.com.  Jokertekens-certificaten worden ook geaccepteerd.

* **Waarom Word ik gevraagd om referenties opnieuw op de pagina van de WAP-server?**

    Als de referenties die u opgeeft voor het verbinden met AD FS-servers geen ook de bevoegdheden voor het beheren van de WAP-servers hebt, klikt u vervolgens Azure AD Connect wordt gevraagd om referenties met Administrator-bevoegdheden op de WAP-servers.

* **De server wordt weergegeven als offline zijn. Wat moet ik doen?**

    Azure AD Connect kan bewerking niet uitvoeren als de server offline is. Als de server deel van de AD FS-farm uitmaakt, controleert u de verbinding met de server. Nadat u het probleem hebt opgelost, drukt u op het vernieuwingspictogram om de status van de wizard te werken. Als de server is onderdeel van de farm eerder, maar nu niet meer bestaat, klikt u op **verwijderen** onderhoudt die Azure AD Connect verwijderen uit de lijst met servers. De AD FS-configuratie zelf niet van invloed op de server wordt verwijderd uit de lijst in Azure AD Connect. Als u van AD FS in Windows Server 2016 of hoger, de server blijft in de configuratie-instellingen gebruikmaakt en opnieuw in de volgende keer worden weergegeven wordt de taak uitgevoerd.

* **Kan ik een subset van mijn farmservers bijwerken met het nieuwe SSL-certificaat**

    Ja. U kunt altijd de taak uitvoeren **SSL-certificaat bijwerken** opnieuw om bij te werken van de resterende servers. Op de **servers selecteren voor SSL-certificaat bijwerken** pagina, kunt u de lijst met servers sorteren op **SSL-vervaldatum** eenvoudige toegang tot de servers die nog niet zijn bijgewerkt.

* **Kan ik de server in de vorige uitvoering verwijderd, maar wordt nog steeds wordt weergegeven als offline en weergegeven op de pagina met AD FS-Servers. Waarom is er nog de offline server, zelfs nadat ik deze verwijderd?**

    De server wordt verwijderd uit de lijst in Azure AD Connect verwijdert niet u deze in de AD FS-configuratie. Azure AD Connect verwijst naar AD FS (WindowsServer 2016 of hoger) voor informatie over de farm. Als de server nog steeds aanwezig in de AD FS-configuratie is, wordt het terug in de lijst worden weergegeven.  

## <a name="next-steps"></a>Volgende stappen

- [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)
- [Active Directory Federation Services-beheer en aanpassingen met Azure AD Connect](how-to-connect-fed-management.md)

