---
title: Toegang tot Key Vault achter een firewall | Microsoft Docs
description: Meer informatie over toegang tot Key Vault van een toepassing achter een firewall
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/13/2016
ms.author: ambapat

---
# Toegang tot Key Vault achter een firewall
### V: De clienttoepassing voor mijn sleutelkluis moet zich achter een firewall bevinden. Welke poorten/hosts/IP-adressen moet ik openen voor toegang tot de sleutelkluis?
Als u toegang wilt krijgen tot een sleutelkluis, moet de clienttoepassing voor de sleutelkluis toegang hebben tot meerdere eindpunten voor verschillende functies.

* Authenticatie (via Azure Active Directory)
* Key Vault-beheer (waaronder maken/lezen/bijwerken/verwijderen en het instellen van toegangsbeleid) via Azure Resource Manager
* Via de sleutelkluis die specifiek is voor een eindpunt (bijvoorbeeld [https://naamvanuwkluis.vault.azure.net](https://yourvaultname.vault.azure.net)) hebt u toegang tot objecten (sleutels en geheimen) die in de sleutelkluis zelf zijn opgeslagen en kunt u ze beheren.  

Er zijn enkele verschillen, afhankelijk van uw configuratie en omgeving.   

## Poorten
Al het verkeer naar de sleutelkluis voor de drie functies (authenticatie, beheer en toegang tot gegevenslaag) gaat via HTTPS: poort 443. Voor CRL gaat het verkeer soms via HTTP (poort 80). Clients die ondersteuning bieden voor OCSP, mogen CRL niet bereiken, maar kunnen soms [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl) bereiken.  

## Authentication
De Key Vault-clienttoepassing moet toegang hebben tot Azure Active Directory-eindpunten voor authenticatie. Welk eindpunt wordt gebruikt, hangt af van de configuratie van de AAD-tenant en het type principal (gebruikers-principal, service-principal) en het type account, bijvoorbeeld Microsoft-account of organisatie-id.  

| Type principal | Eindpunt:poort |
| --- | --- |
| Gebruiker gebruikt Microsoft-account<br> (bijvoorbeeld gebruiker@hotmail.com) |**Wereldwijd:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure van de Amerikaanse overheid:**<br> login-us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> login.microsoftonline.de:443<br><br> en <br>login.live.com:443 |
| Gebruikers-/service-principal gebruikt organisatie-id met AAD (bijvoorbeeld gebruiker@contoso.com) |**Wereldwijd:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure van de Amerikaanse overheid:**<br> login-us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> login.microsoftonline.de:443 |
| Gebruikers-/service-principal gebruikt organisatie-id plus ADFS of ander federatief eindpunt (bijvoorbeeld gebruiker@contoso.com) |Alle bovenstaande eindpunten voor organisatie-id plus ADFS of andere federatieve eindpunten |

Er zijn andere, mogelijk complexe scenario's. Raadpleeg [Azure Active Directory Authentication Flow](/documentation/articles/active-directory-authentication-scenarios/) (Azure Active Directory-authenticatiestroom), [Integrating Applications with Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) (Toepassingen integreren met Azure Active Directory) en [Active Directory Authentication Protocols](https://msdn.microsoft.com/library/azure/dn151124.aspx) (Active Directory-authenticatieprotocollen) voor meer informatie.  

## Key Vault-beheer
Voor Key Vault-beheer (CRUD en toegangsbeleid instellen), moet de clienttoepassing voor de sleutelkluis toegang hebben tot het Azure Resource Manager-eindpunt.  

| Type bewerking | Eindpunt:poort |
| --- | --- |
| Bewerkingen voor de controlelaag van Key Vault<br> via Azure Resource Manager |**Wereldwijd:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Graph API |**Wereldwijd:**<br> graph.windows.net:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> graph.windows.net:443<br><br> **Azure Duitsland:**<br> graph.cloudapi.de:443 |

## Key Vault-bewerkingen
Voor alle beheer- en cryptografiebewerkingen van het sleutelkluisobject (sleutels en geheimen) moet de sleutelkluisclient toegang hebben tot het eindpunt van de sleutelkluis. Het DNS-achtervoegsel van het eindpunt verschilt, afhankelijk van de locatie van Key Vault. Het eindpunt van Key Vault heeft de indeling <kluisnaam>.<regiospecifiek-dns-achtervoegsel>, zoals beschreven in de onderstaande tabel.  

| Type bewerking | Eindpunt:poort |
| --- | --- |
| Key Vault-bewerkingen zoals cryptografiebewerkingen voor sleutels, het maken/lezen/bijwerken/verwijderen, het instellen/ophalen van tags en andere kenmerken voor sleutelkluisobjecten (sleutels/geheimen) |**Wereldwijd:**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 |

## IP-adresbereiken
In de Key Vault-service worden andere Azure-resources gebruikt, zoals de PaaS-infrastructuur. Het is daarom niet mogelijk om een specifiek bereik van IP-adressen voor eindpunten van de sleutelkluisservice te geven. Raadpleeg het document [Microsoft Azure Datacenter IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) (Microsoft Azure Datacenter IP-adresbereiken) als met uw firewall alleen IP-adresbereiken worden ondersteund.   Voor authenticatie en identiteit (Azure Active Directory) moet uw toepassing verbinding kunnen maken met de eindpunten beschreven in [Authentication and identity Addresses](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) (Authenticatie en identiteit van adressen).

## Volgende stappen
* Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Key Vault.

<!--HONumber=Sep16_HO3-->


