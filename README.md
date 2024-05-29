# SickOS1.1_Write-up
I've completed the gaining root level access on SickOS1.1 machine, let's get started.
After importing machine to Vmware i run arp-scan tool to discover the IP address of the target.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/1b5c38fd-eb09-44a1-a7e4-a3529a74b2db)
As shown in the picture our target has the ip address 192.168.146.129
Hence i started a nmap scan to find out open ports and running services.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/01cc4e12-bfef-4a94-ba6a-f21b9cb9b319)
it seems just ssh and proxy ports are running. Let's configure our web browser to use this.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/13c836dc-f108-441e-8a48-bba8cde7b6ba)

Then, when we request the main page, we don't see any thing important.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/32addf84-a0f6-434c-9643-9259db651473)
 Let's give this page to nikto. We have to give the proxy option to nikto.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/615f4bd3-0ab6-4c77-b320-6c3796f28780)
output says, robots.txt file exist and server is vulnerable to Shellshock.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/f722d9b8-22f9-47c8-9f3c-65890c03a4b7)
 Let's visit wolfcms to view the content.
 ![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/f6da96ed-e48f-4caf-8747-020ee5b78746)
It seems server is using a lightweight opensource content management system wolfcms.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/2b43adea-ba32-4819-9b8d-5c6396f37a80)
This kind of systems must have an admin panel to manage the content, i decided to find this. I used dirbuster but it didnt work.
So i googled the issue and was able to find login page for management.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/087d63cf-3be5-42f2-a513-23e67a29ec12)
let's try.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/5236f272-ec69-43b5-933b-ca0362fc4143)
Quickly i tried admin/admin and suprisingly it worked.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/84e1e755-1815-4441-a4e9-00bbab563cea)

now we can upload our webshell, I used and edited the pentestmonkey's one located ad /usr/share/webshells/php/reverse-shell.php
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/549dbd27-6a4f-4e69-abc8-9ad0df999201)
 I uploaded this file(reverse.php) and started a netcat listener. now We have a shell with limited user.
 ![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/cb12fc0e-2d67-4390-9204-3541f050fb58)

![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/990f4e0d-2d31-46f8-919c-ce8493810039)

in the folder /var/www/wolfcms/public i saw a config.php file. here found mysql root credentials (root/john@123)

![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/08b758f1-850e-48c2-a8e0-133424291eae)

when we view the /etc/passwd file, also there is a sickos user on the system.
i tried this password for this user and it worked.
![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/045e4f74-d00c-4c79-abb2-e5708c0844dd)

![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/5c639318-896b-41da-902e-c3d99c0daae1)

![image](https://github.com/harunsvnc/SickOS1.1_Write-up/assets/75423540/9ffb2617-34ed-4c32-85f1-693ee01eb531)


and here is the flag. thanks for @vulnhub and @d4rk for this machint.
