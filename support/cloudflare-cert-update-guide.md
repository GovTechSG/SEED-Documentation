<!-- !> This documentation is obsolete. Refer to [SHIP-HATS documentation](https://docs.developer.tech.gov.sg/docs/ship-hats-docs/) for more details. -->

# Cloudflare Certificate Update Guide

This article guides you to update the Cloudflare Cert on your GMD

## Download Cloudflare Root Certificates

Click on the links to download the certifcate to your GMD.

| Environment | Certificate | Validity |
| --- | --- |--- |
| Cloudflare PROD | [Cloudflare_CA.crt](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.crt)<br>[Cloudflare_CA.pem](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem) | Currently active until 26 Dec 2029 |
| Cloudflare DEV | [Cloudflare_CA _dev.crt](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA_dev.crt)<br>[Cloudflare_CA_dev.pem](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA_dev.pem) | Currently active 18 Dec 2029 |
| Cloudflare STG |[Cloudflare_CA _stg.crt](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA_stg.crt)<br>[Cloudflare_CA_stg.pem](https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA_stg.pem) | Currently active 23 Dec 2029 |

## Certificate update for macOS

To install a Cloudflare certificate in macOS, you need to download a certificate in .crt format.
1.	Download a Cloudflare certificate.
2.	Open the .crt file in Keychain Access. If prompted, enter your local password.
3.	In **Keychain**, choose the access option that suits your needs and select **Add**.
4.	In the list of certificates, locate the newly installed certificate. Keychain Access will mark this certificate as not trusted. Right-click the certificate and select **Get Info**.
5.	Select **Trust**. Under **When using this certificate**, select _Always Trust_.
The root certificate is now installed and ready to be used.

## Certificate update for Windows

1.	Download a Cloudflare certificate.
2.	Right-click the certificate file.
3.	Select **Open**. If a security warning appears, choose **Open** to proceed.
4.	The **Certificate** window will appear. Select **Install Certificate**.
5.	Now choose a Store Location. If a security warning appears, choose **Yes** to proceed.
6.	On the next screen, select **Browse**.
7.	In the list, choose the _Trusted Root Certification Authorities_ store.
8.	Select **OK**, then select **Finish**.
The root certificate is now installed and ready to be used.

## Verify Cloudflare root certification

### For macOS users

Follow these steps to verify if the Cloudflare root certificate is successfully installed using Keychain Access:

1. Open the **Keychain Access** app on your Mac:
   - Press **Command + Space bar**, type **Keychain Access**, and press **Return**.
2. Enter your MacBook's password if prompted.
3. In Keychain Access, click **System** on the left sidebar.
4. Look for the Cloudflare certificate titled **Gateway CA - Cloudflare Managed G1 d4cab1c0e006138441e1f1b57bfde614**.
5. Ensure the expiry date of the certificate is **26 December 2029**.

### For Windows users

To verify SSL certificates on Windows, use the Certificate Manager tool:

1. Open the command prompt.
2. Type `certlm.msc` and press **Enter**.
3. Expand the folder **Trusted Root Certification Authorities > Certificates**.
4. Locate the Cloudflare certificate titled **Gateway CA - Cloudflare Managed G1 d4cab1c0e006138441e1f1b57bfde614**.
5. Verify that the expiry date of the certificate is **26 December 2029**.


> **Note**:
>- Link to: [Configuration of Common Developer CLI tools with Cloudflare WARP](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/configuration-of-common-developer-cli-tools-with-cloudflare-warp-guide)
<br><br>Once you have downloaded the respective certificate, you can configure your Developer CLI tools to trust the Cloudflare Certificate.
