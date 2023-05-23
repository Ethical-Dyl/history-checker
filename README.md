# history-checker

# Initial Enumeration 
My nmap scans had shown there are three ports open.
The ports being 21,22,80 (ftp,ssh,http).

<img width="472" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/e3cc805b-0acd-4e7b-b211-4bc76d1dfa26">

# Web Enumeration # 

I will begin with adding the VM to my host files and running a gobuster scan against it.
After it concludes there is a directory named blogs, while checking that out I will run another gobuster scan against the directory to check for subdirectories while I check out that page.

<img width="767" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/78bc671f-0d98-43a5-be6e-4829a847f581">

After navigating and inspecting the page I see there is a subdirectory left as a hint:

<img width="736" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/dfd8688e-827c-42a2-a5db-b86e3c0051ec">

After navigating to the page and inspecting it once again I see there is a secret key hidden in the page. 

<img width="909" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/0e864dbb-662a-476d-92a2-48e8452358db">


# FTP Enumeration # 
Beginning with an attempt to connect anonomously worked, in doing so I found there was a file called "trytofind.jpg"
Downloading it to my machine will allow me to further inspect it.

<img width="959" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/380ef82a-f2a4-4ef1-a99a-a18b9e19bc4f">

Afer opening the image I see a really cool picture of a haxor_cat with a HTP banner in the back.

<img width="441" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/78d3473e-1f7c-4238-bbee-ee12f2388bb9">

# Stentenogrophy #

Using steghide I tried to extract any files there may be in the image however, the image requires a passphrase. 

<img width="345" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/68b1186b-5113-49f6-83c7-905bf6ed1377">


Using the passphrase found in the secret page I was able to download a file called data.txt that had a user named renu being named, I will be adding this for remembrance, the file also mentions that his password is very weak, likely being in a password list I will now try to bruteforce his credentials.

<img width="552" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/ce5b50a0-9026-4a1c-974b-34380ba57059">

# SSH # 

Using renu's username and a password list from SecList I was able to sucessfully bruteforce his password utilizing the hydra tool.

<img width="956" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/1b963df0-4169-46e9-bec7-274b4b4a540b">

renu deciding on a good password:
<img width="90" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/189e444e-6bc5-443e-8c13-d5b7cd50283d">


# Initial Access # 

Now that I am in the machine I have the first flag and I will do some snooping on what permissions this user has as well as their bash history.

<img width="379" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/c27919a8-fb52-4639-9517-d8c0e755b330">


<img width="306" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/5b9db233-ec10-44e9-a443-ff65307ad262">


It appears there is another user Lilly that shares the same private key for ssh access, after attempting to ssh into the localbox from renu to lilly using the same id_rsa it worked! 

<img width="498" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/6a4d37e7-88b4-43b2-8a0f-baad7e2f8018">

After checking the permissions of the lily user and grabbing the second flag, I see that using perl I can escalate my privileges to root! 

# Privilege Escalation # 
Using a quick exec command I was able to get root and nab the last flag! 

<img width="479" alt="image" src="https://github.com/Ethical-Dyl/history-checker/assets/66540055/3594991f-0a37-409f-b492-4da9576d1252">


# Final thoughts # 

This was a really fun way to kick back relax and do some fun tricks. It showcases the importance of a few major weaknesses that occurs in many workplaces today.
Using weak and easily crackable passwords, unsecured permissions (especially running as root!), clearing out commands that could lead to other accounts or your entire system becoming compromised.

Thank you to Kirthik-KarvendhanT for this fun box!
