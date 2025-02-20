# Office Visio Activcation

#### First method: Activate Visio 2019 with the help of the Command Prompt

**Step 1**. Open the start menu and search for CMD. It's necessary to start the command prompt with administrator privileges.

<figure><img src="https://activateforfree.com/wp-content/uploads/2020/08/run-cmd-elevated-permissions.png" alt=""><figcaption></figcaption></figure>

Default location where Visio 2019 is installed:

**For 32-bit version**:  "%ProgramFiles(x86)%\Microsoft Office\Office16"

**For 64-bit version**: "%ProgramFiles%\Microsoft Office\Office16"

**Step 2**. With the use of **cd** command you can change your path to the one where Visio 2019 files are located.

cd "%ProgramFiles%\Microsoft Office\Office16"

<figure><img src="https://activateforfree.com/wp-content/uploads/2020/08/command-line-office16-folder.png" alt=""><figcaption></figcaption></figure>

Before proceeding with the activation it's necessary to convert the version from Retail to Volume. This will allow us to connect later to a KMS Server and proceed to validate the software.

**Step 3**. Using the **ospp.vbs** script you can install the required licenses to perform the conversion.

Please copy and execute these commands one by one.

```bash
cscript ospp.vbs /inslic:"..\root\Licenses16\VisioPro2019VL_KMS_Client_AE-ppd.xrm-ms"
cscript ospp.vbs /inslic:"..\root\Licenses16\VisioPro2019VL_KMS_Client_AE-ul.xrm-ms"
cscript ospp.vbs /inslic:"..\root\Licenses16\VisioPro2019VL_KMS_Client_AE-ul-oob.xrm-ms"
```

<figure><img src="https://activateforfree.com/wp-content/uploads/2020/08/installing-office-2019-license.png" alt=""><figcaption></figcaption></figure>

**Step 4**. Now your version has been successfully converted to Volume. You can proceed to install a product key to validate the software.

Type this into the console:

```bash
cscript ospp.vbs /inpkey:9BGNQ-K37YR-RQHF2-38RQ3-7VCBB
```

<figure><img src="https://activateforfree.com/wp-content/uploads/2020/08/office-product-key-install.png" alt=""><figcaption></figcaption></figure>

**Step 5**. Now we need to specify the KMS server we will use to validate our product key

You can set the host to **kms8.msguides.com** using this command. Alternatively you can configure **kms.digiboy.ir**

```bash
cscript ospp.vbs /sethst:kms8.msguides.com
```

**Step 6**. We can proceed with the activation of Visio 2019

```bash
cscript ospp.vbs /act
```

If everything's fine, you will receive a confirmation in the console that the operation was successful. Now your software is currently activated!

You can now fully enjoy the functionalities of Visio 2019

Using **cscript ospp.vbs /dstatus** we can check for the remaining grace period since the last validation with the KMS Server
