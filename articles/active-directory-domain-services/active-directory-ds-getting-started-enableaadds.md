---
title: 'Azure Active Directory Domain Services: Azure Active Directory Domain Services inschakelen | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: e5f1fe51d8931985fa55b2d8c0a3fd25bb93f20f
ms.lasthandoff: 04/12/2017


---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services inschakelen
## <a name="task-3-enable-azure-active-directory-domain-services"></a>Taak 3: Azure Active Directory Domain Services inschakelen
In deze taak schakelt u Azure Active Directory Domain Services (Azure AD DS) in voor uw directory door de volgende handelingen uit te voeren:

1. Ga naar de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Selecteer de knop **Active Directory** in het linkerdeelvenster.
3. Selecteer de Azure Active Directory-tenant (Azure AD) (directory) waarvoor u Azure AD DS wilt inschakelen.

    ![Azure AD-directory selecteren](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klik op de pagina **Directory bekijken** op het tabblad **Configureren**.

    ![Tabblad Configureren van directory](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Wijzig onder **Domain Services** de optie **Domain Services inschakelen voor deze directory** in **Ja**.  
    Er worden extra configuratieopties voor Azure Active Directory Domain Services weergegeven op de pagina.

    ![Domain Services inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Wanneer u Azure Active Directory Domain Services inschakelt voor uw tenant, worden de referentie-hashes van Kerberos en NTLM die nodig zijn voor de verificatie van gebruikers, gegenereerd en opgeslagen door Azure AD.
   >
   >
6. Geef de **DNS-domeinnaam van domeinservices** op.

   * De standaarddomeinnaam van de directory (met het achtervoegsel **.onmicrosoft.com**) wordt standaard geselecteerd.

   * De lijst bevat alle domeinen die zijn geconfigureerd voor uw Azure AD-directory, inclusief de geverifieerde en niet-geverifieerde domeinen die u configureert op het tabblad **Domeinen**.

   * U kunt ook een aangepaste domeinnaam opgeven. In dit voorbeeld is de aangepaste domeinnaam *contoso100.com*.

     > [!WARNING]
     > Het voorvoegsel van de opgegeven domeinnaam (bijvoorbeeld *contoso100* in de domeinnaam *contoso100.com*) mag maximaal 15 tekens bevatten. U kunt geen Azure Active Directory Domain Services-domein maken met een voorvoegsel van meer dan 15 tekens.
     >
     >
7. Controleer of de DNS-domeinnaam die u hebt gekozen voor het beheerde domein, nog niet bestaat in het virtuele netwerk. Controleer met name het volgende:

   * Of het virtuele netwerk al een domein met dezelfde DNS-domeinnaam bevat.

   * Of het virtuele netwerk dat u hebt geselecteerd, een VPN-verbinding heeft met uw on-premises netwerk en u een domein met dezelfde DNS-domeinnaam hebt op uw on-premises netwerk.

   * Of het virtuele netwerk een bestaande cloudservice met deze naam bevat.
8. Selecteer een virtueel netwerk waarop Azure Active Directory Domain Services beschikbaar moet zijn. Selecteer het virtuele netwerk en toegewezen subnet dat u hebt gemaakt, in de vervolgkeuzelijst **Domeinservices verbinden met dit virtuele netwerk**. Houd ook rekening met het volgende:

   * Controleer of het virtuele netwerk dat u hebt opgegeven, deel uitmaakt van een Azure-regio die wordt ondersteund door Azure Active Directory Domain Services. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure Active Directory Domain Services beschikbaar is.

   * Virtuele netwerken die bij een regio horen waarin Azure Active Directory Domain Services niet wordt ondersteund, worden niet weergegeven in de vervolgkeuzelijst.

   * Gebruik een toegewezen subnet in het virtuele netwerk voor Azure Active Directory Domain Services. Selecteer *niet* het gatewaysubnet. Zie [Aandachtspunten voor netwerken](active-directory-ds-networking.md).

   * Virtuele netwerken die zijn gemaakt met Azure Resource Manager, worden ook niet weergegeven in de vervolgkeuzelijst. Virtuele netwerken op basis van Resource Manager worden momenteel niet ondersteund door Azure Active Directory Domain Services.
9. Klik op **Opslaan** in het taakvenster onder aan de pagina om Azure Active Directory Domain Services in te schakelen. 
    * Terwijl Azure Active Directory Domain Services wordt ingeschakeld voor uw directory, wordt op de pagina de status *In behandeling* weergegeven.

        ![Het venster Domain Services inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Azure Active Directory Domain Services zorgt voor maximale beschikbaarheid voor uw beheerde domein. Nadat u Azure Active Directory Domain Services hebt ingeschakeld, ziet u dat de IP-adressen waarop domeinservices beschikbaar zijn, een voor een worden weergegeven in het virtuele netwerk. Het tweede IP-adres wordt kort na het eerste weergegeven, zodra maximale beschikbaarheid is ingeschakeld voor uw domein. Wanneer maximale beschikbaarheid is geconfigureerd en actief is voor uw domein, ziet u twee IP-adressen in de sectie **Domeinservices** van het tabblad **Configureren**.
        >
        >
    * Na 20 tot 30 minuten ziet u het eerste IP-adres waarop domeinservices beschikbaar zijn in het virtuele netwerk in het veld **IP-adres** op de pagina **Configureren**.

        ![Het venster Domain Services met het eerste IP-adres dat is ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Wanneer maximale beschikbaarheid is ingeschakeld voor uw domein, worden er twee IP-adressen weergegeven op de pagina. Het beheerde domein is op deze twee IP-adressen beschikbaar in het geselecteerde virtuele netwerk. 
    
10. Noteer de twee IP-adressen, zodat u de DNS-instellingen voor het virtuele netwerk kunt bijwerken. Hierdoor kunnen virtuele machines in het virtuele netwerk verbinding maken met het domein voor bewerkingen, zoals het koppelen van domeinen.

    ![Het venster Domain Services met beide ingerichte IP-adressen](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Afhankelijk van de grootte van de Azure AD-tenant (bijvoorbeeld het aantal gebruikers of groepen) kan de synchronisatie met het beheerde domein enige tijd duren. Dit synchronisatieproces wordt op de achtergrond uitgevoerd. Voor grote tenants met tienduizenden objecten kan het een dag of twee duren voordat alle gebruikers, groepslidmaatschappen en referenties zijn gesynchroniseerd.
>
>

## <a name="next-steps"></a>Volgende stappen
Taak 4: [DNS-instellingen bijwerken voor het virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)

