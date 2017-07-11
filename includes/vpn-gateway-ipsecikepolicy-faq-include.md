<a id="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus" class="xliff"></a>

### Wordt het aangepaste beleid voor IPsec/IKE op alle Azure VPN Gateway-SKU's ondersteund?
Het aangepaste beleid voor IPsec/IKE wordt ondersteund op Azure VPN-gateways **VpnGw1, VpnGw2, VpnGw3, Standard** en **HighPerformance**. SKU **Basic** wordt NIET ondersteund.

<a id="how-many-policies-can-i-specify-on-a-connection" class="xliff"></a>

### Hoeveel beleidsregels kan ik opgeven voor een verbinding?
U kunt maar ***één*** beleidscombinatie opgeven voor een bepaalde verbinding.

<a id="can-i-specify-a-partial-policy-on-a-connection-eg-only-ike-algorithms-but-not-ipsec" class="xliff"></a>

### Kan ik een gedeeltelijk beleid opgeven voor een verbinding? (Bijvoorbeeld alleen IKE-algoritmen maar niet IPsec-algoritmen)
Nee, u moet alle algoritmen en parameters opgeven voor zowel IKE (Main Mode) en IPsec (Quick Mode). Gedeeltelijke beleidsspecificatie is niet toegestaan.

<a id="what-are-the-algorithms-and-key-strengths-supported-in-the-custom-policy" class="xliff"></a>

### Wat zijn de algoritmen en belangrijkste sterke punten die in het aangepaste beleid worden ondersteund?
De volgende tabel bevat de ondersteunde cryptografische algoritmen en belangrijkste sterke punten die door klanten kunnen worden geconfigureerd. U moet voor elk veld een optie selecteren.

| **IPsec/IKEv2**  | **Opties**                                                                 |
| ---              | ---                                                                         |
| IKEv2-versleuteling | AES256, AES192, AES128, DES3, DES                                           |
| IKEv2-integriteit  | SHA384, SHA256, SHA1, MD5                                                   |
| DH-groep         | ECP384, ECP256, DHGroup24, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, geen |
| IPsec-versleuteling | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, geen    |
| IPsec-integriteit  | GCMAES256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                          |
| PFS-groep        | ECP384, ECP256, PFS24, PFS2048, PFS14, PFS2, PFS1, geen                     |
| QM SA-levensduur*  | Seconden (geheel getal; **min. 300**) en kB (geheel getal; **min. 1024**)                                      |
| Verkeersselector | UsePolicyBasedTrafficSelectors** ($True/$False; standaard $False)                             |
|                  |                                                                             |

* (*) SA-levensduur voor IKEv2 Main Mode staat vastgesteld op 28.800 seconden op de Azure VPN-gateways
* (**) Zie het volgende FAQ-item voor "UsePolicyBasedTrafficSelectors"

<a id="does-everything-need-to-match-between-the-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations" class="xliff"></a>

### Moeten het beleid van de Azure VPN-gateway en de configuraties van mijn on-premises VPN-apparaat volledig overeenkomen?
De configuratie van uw on-premises VPN-apparaat moet overeenkomen met of de volgende algoritmen en parameters bevatten die u in het Azure IPsec/IKE-beleid opgeeft:

* IKE-versleutelingsalgoritme
* IKE-integriteitsalgoritme
* DH-groep
* IPsec-versleutelingsalgoritme
* IPsec-integriteitsalgoritme
* PFS-groep
* Verkeersselector (*)

De SA-levensduren zijn alleen lokale specificaties en hoeven niet overeen te komen.

Als u **UsePolicyBasedTrafficSelectors** inschakelt, moet u ervoor zorgen dat voor uw VPN-apparaat dezelfde verkeersselectoren zijn gedefinieerd voor alle combinaties van de voorvoegsels van uw lokale netwerk (lokale netwerkgateway) naar/vanuit de voorvoegsels van het virtuele Azure-netwerk, in plaats van any-to-any. Als uw lokale netwerkvoorvoegsels bijvoorbeeld 10.1.0.0/16 en 10.2.0.0/16 zijn, en de voorvoegsels van uw virtuele netwerk 192.168.0.0/16 en 172.16.0.0/16, moet u de volgende verkeersselectoren opgeven:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Raadpleeg [Connect multiple on-premises policy-based VPN devices](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (Verbinding maken met meerdere on-premises, op beleid gebaseerde VPN-apparaten) voor meer informatie over hoe u deze optie kunt gebruiken.

<a id="does-the-custom-policy-replace-the-default-ipsecike-policy-sets-for-azure-vpn-gateways" class="xliff"></a>

### Vervangt het aangepaste beleid de standaard IPsec/IKE-beleidssets voor Azure VPN-gateways?
Ja, zodra een aangepast beleid is opgegeven voor een verbinding, gebruikt de Azure VPN-gateway alleen het beleid op de verbinding, zowel als IKE-initiator als IKE-beantwoorder.

<a id="if-i-remove-a-custom-ipsecike-policy-does-the-connection-become-unprotected" class="xliff"></a>

### Als ik een aangepast beleid voor IPsec/IKE verwijder, wordt de verbinding dan onbeveiligd?
Nee, de verbinding wordt nog steeds beveiligd met IPsec/IKE. Wanneer u het aangepaste beleid van een verbinding verwijdert, wordt de Azure VPN-gateway teruggezet naar de [standaardlijst met IPsec/IKE-voorstellen](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) en wordt de IKE-handshake opnieuw gestart met uw on-premises VPN-apparaat.

<a id="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection" class="xliff"></a>

### Kan het toevoegen of bijwerken van een beleid voor IPsec/IKE mijn VPN-verbinding verstoren?
Ja, dit kan tot een onderbreking van een paar seconden leiden, omdat de Azure VPN-gateway de bestaande verbinding verbreekt en de IKE-handshake opnieuw start om de IPsec-tunnel opnieuw in te stellen met de nieuwe cryptografische algoritmen en parameters. Zorg ervoor dat uw on-premises VPN-apparaat met dezelfde algoritmen en sterke punten wordt geconfigureerd om de onderbreking te minimaliseren.

<a id="can-i-use-different-policies-on-different-connections" class="xliff"></a>

### Kan ik verschillende beleidsregels voor verschillende verbindingen gebruiken?
Ja. Aangepast beleid wordt per verbinding toegepast. U kunt verschillende IPsec/IKE-beleidsregels toepassen op verschillende verbindingen. U kunt ook aangepaste beleidsregels toepassen op een subset van verbindingen. Voor de resterende verbindingen worden de standaard IPsec/IKE-beleidssets voor Azure toegepast.

<a id="can-i-use-the-custom-policy-on-vnet-to-vnet-connection-as-well" class="xliff"></a>

### Kan ik het aangepaste beleid ook voor een VNet-naar-VNet-verbinding gebruiken?
Ja, u kunt een aangepast beleid toepassen op zowel cross-premises IPsec-verbindingen als VNet-naar-VNet-verbindingen.

<a id="do-i-need-to-specify-the-same-policy-on-both-vnet-to-vnet-connection-resources" class="xliff"></a>

### Moet ik hetzelfde beleid opgeven voor beide VNet-naar-VNet-verbindingsresources?
Ja. Een VNet-naar-VNet-tunnel bestaat uit twee verbindingsresources in Azure, één voor elke richting. Beide verbindingsresources moeten hetzelfde beleid hebben, anders kan de VNet-naar-VNet-verbinding niet tot stand worden gebracht.

<a id="does-custom-ipsecike-policy-work-on-expressroute-connection" class="xliff"></a>

### Werkt een aangepast IPsec/IKE-beleid op een ExpressRoute-verbinding?
Nee. IPsec/IKE-beleid werkt alleen op S2S-VPN- en VNet-naar-VNet-verbindingen via de Azure VPN-gateways.
