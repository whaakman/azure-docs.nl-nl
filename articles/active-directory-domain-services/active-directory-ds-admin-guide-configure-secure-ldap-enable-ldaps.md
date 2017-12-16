---
title: Beveiligde LDAP (LDAPS) in Azure AD Domain Services configureren | Microsoft Docs
description: Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: maheshu
ms.openlocfilehash: d55abe651f69e3539e7584b40a7aedf419bccda1
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren

## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u hebt voltooid [taak 2: het beveiligde LDAP-certificaat te exporteren een. PFX-bestand](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal"></a>Taak 3: het inschakelen van beveiligde LDAP voor het beheerde domein met de Azure-portal
Om veilige LDAP, moet u de volgende configuratiestappen uitvoeren:

1. Navigeer naar de  **[Azure-portal](https://portal.azure.com)**.

2. Zoek naar 'domeinservices' in de **zoeken bronnen** zoekvak. Selecteer **Azure AD Domain Services** uit de zoekresultaten. De **Azure AD Domain Services** pagina worden uw beheerde domein.

    ![Zoeken naar beheerde domein wordt ingericht](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klik op de naam van het beheerde domein (bijvoorbeeld, contoso100.com') voor meer informatie over het domein.

    ![Domain Services - status voor inrichten heeft](./media/getting-started/domain-services-provisioning-state.png)

3. Klik op **beveiligde LDAP** in het navigatiedeelvenster.

    ![Domain Services - pagina van de beveiligde LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Beveiligde LDAP-toegang tot uw beheerde domein is standaard uitgeschakeld. Wisselknop **beveiligde LDAP** naar **inschakelen**.

    ![Beveiligde LDAP inschakelen](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Beveiligde LDAP-toegang tot uw beheerde domein via internet is standaard uitgeschakeld. Wisselknop **beveiligde LDAP toegang via internet toestaan** naar **inschakelen**, indien gewenst. 

    > [!WARNING]
    > Wanneer u beveiligde LDAP toegang via internet inschakelt, is uw domein is vatbaar voor wachtwoord gewelddadige aanvallen via internet. Daarom raden we instellen van een NSG toegang tot de vereiste bron-IP-adresbereiken vergrendelen. Zie de instructies voor het [vergrendelen LDAPS toegang tot uw beheerde domein via internet](#task-5---lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet).
    >

6. Klik op het pictogram map volgende **. PFX-bestand met LDAP om certificaten veilig**. Geef het pad naar het PFX-bestand met het certificaat voor beveiligde LDAP-toegang tot het beheerde domein.

7. Geef de **wachtwoord voor het decoderen. PFX-bestand**. U hebt gebruikt bij het exporteren van het certificaat naar het PFX-bestand hetzelfde wachtwoord opgeven.

8. Wanneer u klaar bent, klikt u op de **opslaan** knop.

9. U ziet een melding dat informeert dat u beveiligde LDAP wordt geconfigureerd voor het beheerde domein. Totdat deze bewerking voltooid is, kunt u andere instellingen voor het domein niet wijzigen.

    ![Beveiligde LDAP voor het beheerde domein configureren](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Het duurt ongeveer 10 tot 15 minuten beveiligde LDAP voor uw beheerde domein inschakelen. Als de opgegeven beveiligde LDAP-certificaat komt niet overeen met de vereiste criteria, beveiligde LDAP is niet ingeschakeld voor uw directory en wordt er een fout. Bijvoorbeeld, de domeinnaam is onjuist, het certificaat is al verlopen of verloopt binnenkort. In dit geval, probeer het opnieuw met een geldig certificaat.
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Taak 4: configureer DNS voor toegang tot het beheerde domein van het internet
> [!NOTE]
> **Optionele taak** : als u niet van plan bent om te krijgen van het beheerde domein met LDAPS via internet, overslaan van deze taak voor de configuratie.
>
>

Voordat u deze taak begint, moet u de stappen in hebben voltooid [taak 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Als u kunt beveiligde LDAP toegang via het internet hebt ingeschakeld voor uw beheerde domein, moet u DNS bijwerken zodat clientcomputers dit beheerde domein vindt. Aan het einde van de taak 3, een externe IP-adres wordt weergegeven op de **eigenschappen** tabblad **externe IP-adres voor LDAPS toegang**.

Uw externe DNS-provider zo configureren dat de DNS-naam van het beheerde domein (bijvoorbeeld ' ldaps.contoso100.com') naar deze externe IP-adres verwijst. Bijvoorbeeld de volgende DNS-vermelding maken:

    ldaps.contoso100.com  -> 52.165.38.113

Dat is alles: u bent nu klaar om verbinding maken met het beheerde domein met behulp van beveiligde LDAP via internet.

> [!WARNING]
> Houd er rekening mee dat clientcomputers de verlener van het certificaat LDAPS te kunnen verbinding maken is met het beheerde domein met LDAPS moeten vertrouwen. Als u een openbaar vertrouwde certificeringsinstantie (CA) gebruikt, hoeft u niet verder niets te doen omdat deze certificaatverleners clientcomputers worden vertrouwd. Als u een zelfondertekend certificaat gebruikt, installeert u het openbare deel van het zelfondertekend certificaat in het vertrouwde certificaatarchief op de clientcomputer.
>
>


## <a name="task-5---lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Taak 5: het vergrendelen van beveiligde LDAP-toegang tot uw beheerde domein via internet
> [!NOTE]
> Als u LDAPS toegang tot het beheerde domein niet via het internet hebt ingeschakeld, slaat u deze taak voor de configuratie.
>
>

Voordat u deze taak begint, moet u de stappen in hebben voltooid [taak 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Het blootstellen van uw beheerde domein voor LDAPS toegang via internet vertegenwoordigt een beveiligingsrisico. Het beheerde domein bereikbaar is via het internet op de poort die wordt gebruikt voor beveiligde LDAP (dat wil zeggen, poort 636). Daarom kunt u toegang tot het beheerde domein specifieke bekende IP-adressen te beperken. Voor betere beveiliging, een netwerkbeveiligingsgroep (NSG) maken en deze koppelen aan het subnet waarin u Azure AD Domain Services hebt ingeschakeld.

De volgende tabel ziet u een voorbeeld van een NSG die u configureren kunt, als u wilt vergrendelen beveiligde LDAP toegang via internet. Het NSG bevat een reeks regels waarmee binnenkomend beveiligde LDAP toegang via TCP-poort 636 alleen uit een opgegeven set van IP-adressen. De 'DenyAll' standaardregel geldt voor alle binnenkomend verkeer van het internet. De regel NSG LDAPS om toegang te verlenen via het internet vanaf de opgegeven IP-adressen heeft een hogere prioriteit dan de DenyAll NSG-regel.

![Voorbeeld NSG LDAPS om toegang te beveiligen via internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Meer informatie** - [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).

<br>


## <a name="troubleshooting"></a>Problemen oplossen
Als u problemen ondervindt bij het verbinding maken met het beheerde domein met behulp van beveiligde LDAP, voert u de volgende stappen:
* Zorg ervoor dat de keten van de verlener van het beveiligde LDAP-certificaat wordt vertrouwd op de client. U kunt de basiscertificeringsinstantie toevoegen aan het certificaatarchief van vertrouwde basiscertificeringsinstanties op de client de vertrouwensrelatie tot stand brengen.
* Controleer of dat het beveiligde LDAP-certificaat niet is uitgegeven door een tussenliggende certificeringsinstantie die niet standaard op een nieuwe windows-computer wordt vertrouwd.
* Controleer of de LDAP-client (bijvoorbeeld ldp.exe) een verbinding maakt met het beveiligde LDAP-eindpunt met een DNS-naam, niet het IP-adres.
* Controleer of de DNS-naam die van de LDAP-client verbinding met wordt omgezet in het openbare IP-adres voor beveiligde LDAP op het beheerde domein maakt.
* Controleer of dat de beveiligde LDAP-certificaat voor uw beheerde domein heeft de DNS-naam in het onderwerp of de alternatieve onderwerpnamen-kenmerk.
* Als u verbinding via LDAP veilig via internet maakt, controleert u dat het NSG-instellingen voor het virtuele netwerk het verkeer op poort 636 vanaf het internet toestaan.

Als u nog steeds problemen ondervindt verbinding te maken met het beheerde domein met behulp van beveiligde LDAP [Neem contact op met het productteam](active-directory-ds-contact-us.md) voor hulp. De volgende informatie om u te helpen bij het analyseren van het probleem beter omvatten:
* Een schermopname van ldp.exe zodat de verbinding en mislukt.
* Uw Azure AD-tenant-ID en de DNS-domeinnaam van uw beheerde domein.
* Exacte gebruikersnaam die u probeert te binden als.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Groepsbeleid voor een Azure AD Domain Services beheerd domein beheren](active-directory-ds-admin-guide-administer-group-policy.md)
* [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md)
* [Een Netwerkbeveiligingsgroep maken](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
