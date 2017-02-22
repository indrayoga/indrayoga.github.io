#Install Apache,PHP,MySQL di Ubuntu 16.04
Bagi web developer yang ingin meng-*install* ***Apache,PHP,MySQL*** di *ubuntu 16.04*, berikut adalah step step nya :
***Install Apache**
1. buka *shell prompt* anda, di *ubuntu* bisa memakai *terminal* atau *xterm*.
2.ketik perintah ini ***sudo apt install apache2***

**Install PHP**
1. masih dalam *shell prompt* anda, ketik perintah ini : ***sudo apt install php libapache2-mod-php php-mysql***

**Install MySQL**
1. masih dalam *shell prompt* anda, ketik perintah ini : ***sudo apt install mysql-server***, selama proses berjalan, anda akan diminta untuk memasukkan password untuk *user root mysql*.

**Install PHPMyAdmin**
1. untuk menginstall *phpmyadmin* gunakan perintah ini :***sudo apt install phpmyadmin***.

##testing
Sebelum anda memulai mencoba web server anda, ada baiknya kita restart apache kit
*Folder web server* anda biasanya berada di */var/www/html*, anda bisa mencoba *web server*nya, contohnya dengan cara :
1. buat file *phpinfo.php* di folder */var/www/html*.
2. buka file *phpinfo.php* dengan editor, biasanya *gedit*.
3. kemudian isi file *phpinfo.php* dengan baris berikut :
	***<?php
	phpinfo();
	?>***
4. Simpan, kemudian buka *browser* anda, dan akses ke *http://localhost/phpinfo.php*