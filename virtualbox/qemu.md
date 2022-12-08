---
description: RIP
---

# 🪦 QEMU

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption><p>QEMU</p></figcaption></figure>

I felt I had to include a section regarding QEMU since this was the original Virtualization Method planned on being used. QEMU is an open-source emulator that can run OS and programs for one machine on a different machine.&#x20;

### What went Wrong?

Ultimately, I ran into multiple issues while trying to configure QEMU to actually start creating and infecting my VMs. After about two weeks, since the purpose of this project was not solely centered around this aspect, VirtualBox was chosen to take over. This was simply due to the fact VirtualBox has its own GUI and is far less painstaking and time-consuming, at least for me.&#x20;

Two different methods of using QEMU were tried, both by manually creating a GUI and by using an open-source GUI related to QEMU, called QtEmu.

### **QEMU from Command Line**

{% embed url="https://www.qemu.org/download/" %}

Originally, creating a VM with QEMU seemed to be straightforward and a breeze. Unfortunately, it was not.&#x20;

Manually, you first have to add the path of where QEMU is located after it has been downloaded from the QEMU link above.&#x20;

<figure><img src="https://lh3.googleusercontent.com/hHaSSDutChaT9c3YITikBwx68AWdLdl_SiifS3IBL0nnzkt-x7caCqHD5ObCbm_bpU7OXmTZ2LsBD6oj7rdDbuWOcwoELyG8xSwIbNiw3GZCxkIfi6cV_xhIIddKcUjBgLn7Y-ou2j6jOuAUayu_wtWzYivsqdOtV5DxR9q1bSq7aAuebviBWlAdoMPtUQ" alt=""><figcaption><p>Adding a Path for QEMU</p></figcaption></figure>

Create the virtual hard drive with the following command: ^/qemu-img create -f qcow2 ubuntu18.04.img 10

<figure><img src="https://lh4.googleusercontent.com/6_PYu2lIFEnEDYD6n989LxMl41yseSCSTCLSW21saM5aMkdTf_oJ2mtyGTBzhSRw0zelw1py4zjejw4HDXjfav18UoFi175Xy018pR4d-FI-q42jPgNA5nccshPZqo90HA1HWevThaADXscHZ7Cn08ClZuiRthsVsZsWEtY0yS1Fem9x6IBX4hhvI_hANQ" alt=""><figcaption><p>Formatting the VM</p></figcaption></figure>

Start the Ubuntu18.04 Virtual Machine: ^/qemu-system-x86\_64.exe -m 1G -smp 2 -boot order=dc -hda ubuntu20.img -cdrom “e:\ubuntu-18.04.6-desktop-amd64.iso”

<figure><img src="https://lh4.googleusercontent.com/QbZ7YAsv1UFemi2Z7InNuwuWGXZXpRTZ0BK9hnMuUbd63Asx8LZ0n42-OwgTWZ1qCQqZ0g4i_Zv63gJhUXoI5Bg-Pjc3JW19Njon1ZBstAt5Rh3eC1KiwdZBdvKAXeeXM23LrBX09RA3FnhDlxZkwqGRCwO0dBlZJYix1XYJQP110hNirG5D1YSkSJYBPg" alt=""><figcaption><p>Starting the VM</p></figcaption></figure>

The process to manually create a GUI took only a few steps, however, when configured the VM itself was running very slowly. And when I say slowly, I mean to the point where it wasn't possible to get anything done. I was unsuccessful in trying to find a solution. If I had more time or QEMU was a more important aspect, more time would have been allocated to finding better commands than these to create the VM. However, QtEmu was suggested as the preferred method, since manually supposably required more troubleshooting.&#x20;

### **QtEmu**

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>QtEmu</p></figcaption></figure>

{% embed url="https://qtemu.org/" %}

QtEmu was tried as an option when QEMU was running way too slow. However, this also presented itself with issues. Despite using the correct path for all of the file locations, I could not get the VM itself to boot. It would get to the stage of powering up, then nothing would happen. This was even attempted on multiple different computers, one being my personal Windows PC and other Remote Windows and Ubuntu VM.&#x20;

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption><p>Switching to TCG</p></figcaption></figure>

One can only assume what was the issue, as further troubleshooting did not provide much. The article I was using even referenced a way to get around the fact the VM did nothing when turning on. It suggested changing the Code Generator from HAXM to TCG (Tiny Code Generator), which should be able to fix the issue. When this did not work either, the project slightly pivoted to use Virtual Box, which is detailed here:&#x20;

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}
