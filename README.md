1. Instalace Zabbix appliance
Nejprve jsem nainstaloval Zabbix appliance – hotové virtuálky s předinstalovaným Zabbixem. Protože mi na localhostu běžela nějaká služba na portu 80, nemohl jsem použít klasický NAT režim (docházelo by ke kolizi portů). Místo toho jsem nastavil síťový režim "Pouze s hostitelem" (Host-only) a appliance přidělil statickou IP adresu 192.168.56.101. Takhle mám Zabbix webové rozhraní dostupné na jasné adrese a nemusím řešit přesměrování portů.

2. Příprava Ansible prostředí
Dále jsem upravil konfiguraci v repozitáři – konkrétně v souboru ansible_provision.yml jsem opravil URL na svůj GitHub, aby si Ansible správně stahovalo potřebné playbooky a role.

3. Spuštění infrastruktury
Pomocí vagrant up jsem spustil virtuální stroje definované ve Vagrantfilu. Tím se vytvořily instance, které chci monitorovat – v mém případě bastion a další servery.

4. Spuštění Ansible playbooku
Následně jsem spustil Ansible playbook, který zajistil:

instalaci Zabbix agentů na všechny spuštěné virtuálky

jejich správné nakonfigurování a napojení na Zabbix server

5. Nastavení autoregistrace v Zabbixu
V Zabbix webovém rozhraní jsem vytvořil autoregistrační pravidlo. Díky tomu se každý nový stroj, na kterém běží Zabbix agent, automaticky zaregistruje do monitorovacího systému, jakmile se poprvé připojí.

6. Úprava metadat pro autoregistraci
Aby autoregistrace správně fungovala, upravil jsem v playbooku host metadata – konkrétně jsem nastavil, aby se stroje označovaly podle své role (např. bastion, webserver, databáze). Díky tomu je pak můžu v Zabbixu automaticky přiřazovat do příslušných skupin a používat na ně správné šablony.

7. Výsledek
Po dokončení celého procesu se mi bastion (a další stroje) objevil v Zabbixu automaticky, bez nutnosti manuálního přidávání. Monitoring běží a vše je připravené k dalšímu rozšiřování.