If you encounter this error when launching the PnetLab in VMware Workstation:

```plaintext
Virtualized Intel VT-x/EPT is not supported on this platform.
Continue without virtualized Intel VT-x/EPT?
```
You choose "Yes", then you will see the error message:

```plaintext
Virtualized Intel VT-x/EPT is not supported on this platform.
```

*This guide is tested on my machine: Windows 10 Pro 64-bit, CPU Intel 1235U, VMware Workstation Pro 17. Other systems should be similar.*

To fix this issue, follow these steps:

> [!WARNING]
> These fixes primarily disable **WSL (Windows Subsystem for Linux)**. As a result, certain Windows features that rely on WSL or Hyper-V, such as virtual machines using WSL as a hypervisor,  third-party services that require Hyper-V, like Docker Desktop — may stop working. To re-enable WSL, refer to section II [Re-enable Hyper-V for other services](#ii-re-enable-hyper-v-for-other-services).

# Fix the issue by disabling Hyper-V and related features



1. **Open Windows PowerShell with administrator privilege and run**:

```shell
bcdedit /set hypervisorlaunchtype off
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
# Type "N" to not restart the computer yet.

# In case you encounter an error when running the above command, try below command:
# Disable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform
```

2. **Go to Windows Settings** -> Search for `Core Isolation` -> Click on `Core isolation details` -> Turn off `Memory integrity`.

![Core isolation](assets/images/Turn_off-core_isolation.png)

3. **Search for  `Edit group policy` on Windows** search bar -> Computer Configuration -> Administrative Templates -> System -> Device Guard -> Turn on Virtualization Based Security -> Right click -> Edit -> Disable.

![Turn off Virtualization Based Security](assets/images/Local_group_policy.png)

4. **Search for `Turn Windows features on or off` on Windows search bar**. Ensure the following options are unchecked:

- `Hyper-V` -> Need to be unchecked
- `Virtual Machine Platform` -> Need to be unchecked
- `Windows Hypervisor Platform` -> Need to be unchecked

![Windows features](assets/images/Window_features.png)

5. **Restart the computer**

6. **Try to launch the PnetLab again**

The PnetLab VM now should be able to run without the error message. However, since the `Hyper-V` is disabled, you will not be able to run any other Windows services that require `Hyper-V` such as Docker Desktop, WSL2, etc.

References: https://youtu.be/p76EhflJ1l0?si=AdlQtFex8kG9Tvxi



# II. Re-enable Hyper-V for other services

In case you need to re-enable `Hyper-V` (after disabling it from the previous steps) for other services (e.g., Docker Desktop), follow these steps in order:



1. **Run Windows Powershell with administrator privilege and run**:

```shell
bcdedit /set hypervisorlaunchtype auto
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -All
```

2. **Turn ON Memory Integrity (Core Isolation)**

Go to Windows Settings → Search for Core Isolation -> Click Core Isolation Details -> Turn on `Memory integrity`.

3. **Enable Virtualization-Based Security in Group Policy**

Press `Windows + R`, type `gpedit.msc` and hit Enter -> Go to: Computer Configuration → Administrative Templates → System → Device Guard -> Double-click "Turn on Virtualization Based Security" → Set to Enabled.

4.  **Enable features in "Windows Features" menu**

Search for `Turn Windows features on or off` in the Start menu.

Check these items:

✅ Hyper-V

✅ Virtual Machine Platform

✅ Windows Hypervisor Platform

Click OK, and wait for Windows to install them.



5. **Restart your computer**

Once you've completed all of the above, restart your computer to apply the changes.




