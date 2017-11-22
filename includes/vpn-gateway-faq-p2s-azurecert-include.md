[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basis-CA voor een punt-naar-site-verbinding gebruiken?

Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Welke hulpprogramma's kan ik gebruiken om certificaten te maken?

U kunt uw Enterprise PKI-oplossing (uw interne PKI), Azure PowerShell, MakeCert en OpenSSL gebruiken.

### <a name="certsettings"></a>Zijn er instructies voor de instellingen van het certificaat en parameters?

* **Interne PKI/Enterprise PKI-oplossing:** raadpleegt u de stappen voor het [certificaten genereren](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** Zie de [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) artikel voor stappen.

* **MakeCert:** Zie de [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) artikel voor stappen.

* **OpenSSL:** 

    * Wanneer het exporteren van certificaten, zorg er dan voor dat het basiscertificaat converteren naar Base64.

    * Voor het certificaat van de client:

      * Geef de lengte als 4096 bij het maken van de persoonlijke sleutel.
      * Bij het maken van het certificaat voor de *-extensies* parameter opgeven *usr_cert*.