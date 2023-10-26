# macOS offboarding guide

 This guide provides instructions for you to offboard your macOS device onboarded to SEED.

## Audience

- Users who need to offboard their macOS device from SEED.

## Prerequisites

Before you begin, make sure you have the following:

- An active TechPass account
- A SEED onboarded device
- [Optional] Your Intune Device ID 

### How to obtain Intune Device ID

You need your Intune Device ID during the offboarding process. Here is how to find it:

?> **Tip**<br>Click the triangle to view more details about each method.

<details>
<summary>Method 1: Retrieve Intune Device ID from your macOS device</summary>


1. Open the **Terminal** and execute the following commands:

```
intune_id="$(security find-certificate -a /Library/Keychains/System.keychain | egrep -B 4 '\"issu\"<blob>=.+MICROSOFT INTUNE MDM DEVICE CA' | grep alis | cut -d '"' -f 4)"
if [ -z "$intune_id" ]
then
    echo "Intune ID not found"
    return
fi

num_candidates="$(echo "$intune_id" | wc -l | xargs echo -n)"
if [ "$num_candidates" -eq 1 ]
then
    echo "$intune_id"
    return
fi

old_ifs="$IFS"
IFS='\n'
actual_id="Intune ID not found"
curr_latest_end_date_unix=0
while read id
do
    end_date="$(security find-certificate -c "$id" -p /Library/Keychains/System.keychain | openssl x509 -noout -enddate | cut -d '=' -f 2)"
    end_date_unix="$(date -j -f "%b %e %H:%M:%S %Y %Z" "$end_date" "+%s")"
    if [ "$end_date_unix" -ge "$curr_latest_end_date_unix" ]
    then
        actual_id="$id"
        curr_latest_end_date_unix="$end_date_unix"
    fi
done <<< "$intune_id"

IFS="$old_ifs"
echo "$actual_id"
```
2. Note down the Intune Device ID that is displayed on the **Terminal** window.

</details>

<details>
<summary>Method 2: Retrieve Intune Device ID from the TechPass portal</summary>

1. On your non-SE GSIB device, go to the [TechPass portal](https://portal.techpass.gov.sg/secure/account/profile).
2. On the TechPass portal, at the top right, go to your user name and click **My Account**. Your **Profile** details are displayed.
3. Take note of the **Intune Device ID** from the **Profile** page.

![tp-intune-device-id](../images/macos-get-intune-device-id.png)

</details>


<details>
<summary>Method 3: Raise a service request to retrieve Intune Device ID.</summary>

?> **Note**<br>Use this method if you cannot log in to your GMD or TechPass portal.

- [Raise a service request](https://go.gov.sg/seed-techpass-support) to retrieve your Intune Device ID.

</details>


> **Note**:
> For more information, refer to [Offboarding FAQ](/faqs/seed-offboarding-faq).

## Phase A: Offboard device from SEED components

1. Go to **Terminal** and run `mdatp health`.

<!--
![open terminal](../images/macos-open-terminal.png)

![find-org-id](../images/macos-find-org-id-1.png)-->

2. Take note of the value displayed for **org_id**.

![note-org-id](../images/macos-find-org-id-2.png)

3. Refer to the following table and identify your **Defender organisation** and download the respective offboarding package.

  | org_id  | Defender organisation | Offboarding package |
  | ------------- |:-------------:|:-------------:|
  | faa36a5e-2da6-4225-8e27-226177c801a0      | WOG     | [Download offboarding package](https://k3uwa66lu3tj6uxft46666ynhe0uvzor.lambda-url.ap-southeast-1.on.aws/local_wog_mac)    |
  | 49237d71-42ac-425a-a803-881b92cc18ce  | TechPass    | [Download offboarding package](https://k3uwa66lu3tj6uxft46666ynhe0uvzor.lambda-url.ap-southeast-1.on.aws/local_tp_mac)     |
  | 6389e966-e334-461d-86ce-0fed12484620 | Hive | Contact [Hive support](mailto:GDS_DEN@hive.gov.sg) to get the offboarding package. |


!> **Important**
>
> - If your **Defender organisation** is **Hive**, please disregard the remaining steps in this document. Instead, you should obtain the offboarding package from Hive support and unenroll your device from Defender. Refer to the [offboarding FAQs](offboard-device/seed-offboarding-faqs.md) for guidance on unenrolling your device from Defender using the Hive offboarding package.
>   
> - If your **Defender organisation** is either **WOG** or **TechPass**, you should use your TechPass account to download the offboarding package and proceed with the remaining steps.
>   
> - If your **Defender organisation** is **none of the above**, please reach out to the IT support of the organization that provided you with the device for further assistance.


4. Go to the folder where you downloaded the ZIP file and extract the files. You should see the following two files.

![extract-files](../images/macos-extracted-files-for-offboarding.png)

?> **Note**: The file names vary with the organisation.


5. On your **Terminal**, run the following command:

```
sudo mdatp config tamper-protection enforcement-level --value audit
```
6. On your **Terminal**, go to the folder where you extracted the files. For example, if they are in the **Downloads** > **Offboarding_local_wog_mac** folder, go to that folder.

![cd-extracted-folder](../images/macos-cd-downloads.png)

7. Copy the below and run it on the same **Terminal**.

    ```
    sudo chmod +x local_mac_offboarding.sh
    ```

8. When prompted for a **Password**, enter your device password.
9. Copy and run the following command on your **Terminal**.

    ```
    sudo ./local_mac_offboarding.sh
    ```

When you see the following success message on your **Terminal**, you will be automatically directed to the **SEED Offboarding: Device Record Removal** form to submit the Intune Device ID.

![macos-success-message](../images/macos-success-message.png)

!>**Important note**<br> Make sure you complete the steps in Phase B immediately after Phase A. Failure to do so may result in your device update policy reinstalling the latest version of the removed SEED components.

## Phase B: Submit Intune Device ID to remove device record

### Prerequisites

- Successful completion of [Phase A: Offboard device from SEED components](#phase-a-offboard-device-from-seed-components).
- **Intune Device ID**: Generally, when you successfully offboard your device from the SEED components, the Intune Device ID is automatically displayed on the **SEED Offboarding: Device Record Removal** form. If it is not displayed, see [Get Intune Device ID](#how-to-obtain-intune-device-id).
- [Optional] If you had raised a support request with the TechPass and SEED support team to offboard your device from the SEED components, please have the reference number ready as we may need this information.

### Submit Intune Device ID

**To submit Intune Device ID**:

1. Ensure your **Intune Device ID** is displayed on the form. If it is not displayed, provide it.
2. Enter your organizational email address in **Organizational Email Address** and click **Verify**.
3. Enter the OTP.
4. Indicate if you had any issues while completing **Phase A**.
5. [Optional] If you had issues completing **Phase A**, we encourage you to provide the **Support Ticket Number**.
6. Click **Submit**. When this request is processed successfully, we send a notification via email.

![successfully-offboarded-email](../images/macos-successfully-offboarded-email.png)


?> **Additional information**<br>- We require up to 30 minutes to process your server-side offboarding request.<br>- If you are still waiting to receive an email after 30 minutes, please raise a [support request](https://go.gov.sg/seed-techpass-support).



## Device clean-up policy

The device clean-up policy applies to SEED users with TechPass IDs belonging to the TechPass Entra ID. You can identify a TechPass Entra ID account if your TechPass ID's domain is *techpass.gov.sg*. For example, *james_lee@techpass.gov.sg* is associated with the TechPass Entra ID.

The primary objective of this policy is to remove inactive device records from the Intune portal.

> **Note**:
> 
> - The device clean-up policy does **not apply** if your TechPass ID belongs to the **WOG Entra ID**.
> - A TechPass ID in the WOG Entra ID typically aligns with your organizational email address, which is in the format *\<your_name\>@\<acronym for your agency\>.gov.sg*. For example, *peter_wilson@tech.gov.sg*.


### What happens if my GMD is inactive?


If your TechPass ID belongs to the TechPass Entra ID and you have not logged into your GMD for 90 consecutive days, the GMD becomes inactive, and its records are soft deleted from the Intune portal.

It is essential to note that when your device records are soft deleted, it does not wipe or retire the device. Instead, the device record is temporarily removed from Intune.

As a result, SEED administrators will not be able to access details such as the device's health status, and they can no longer manage it from the SEED Dashboard.


### Restore my device records on Intune

You can restore your device records on Intune by simply logging in to your GMD device the next time, provided that:

- Your TechPass account is still active.
- Your MDM certificate is still valid or within 180 days after its expiry.

## MDM certificate

When you onboard your Internet Device to SEED, you receive an MDM certificate that is valid for one year from the date of onboarding. The certificate is automatically renewed if you are logged in to your GMD when it expires.

> **Note**:
> 
> - Ensure that your TechPass account remains active.

If the MDM certificate expires, it can be automatically renewed by logging in to your device within 180 days from the expiration date. In such cases, re-onboarding your device to SEED is not required.

If the certificate remains expired for over 180 days, your device record is permanently deleted, preventing access to SGTS products.





