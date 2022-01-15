import time
import tracemalloc
import argparse
from _socket import gethostbyname, gethostbyaddr, setdefaulttimeout
from socket import socket, AF_INET, SOCK_STREAM
from threading import Thread
def connScanner(tgtHost, tgtPort):
    try:
        connSocket = socket(AF_INET, SOCK_STREAM)
        connSocket.connect((tgtHost,tgtPort))
        connSocket.send(bytes('KnockKnock\r\n', 'UTF-8'))
        results = connSocket.recv(100)
        print('[+]',tgtPort,'open')
        print('    [RESP]', str(results))
    except:
        pass
    finally:
        connSocket.close()
def portScanner(tgtHost, tgtPorts, tgtRange):
    try:
        tgtIP = gethostbyname(tgtHost)
    except:
        print('[-] Cannot resolve', tgtHost, ': Unknown host')
        return
    try:
        tgtName = gethostbyaddr(tgtIP)
        print('\n[+] Scan Results for:', tgtName[0])
    except:
        print('\n[+] Scan Results for:', tgtIP)
    setdefaulttimeout(1)
    if tgtRange == 'None':
        for tgtPort in tgtPorts:
            t = Thread(target=connScanner, args=(tgtHost, int(tgtPort)))
            t.start()
    if tgtPorts[0] == 'None':
        for tgtScopeBegin in range(int(tgtRange)):
            t = Thread(target=connScanner, args=(tgtHost, int(tgtScopeBegin)))
            t.start()
    if tgtRange != 'None' and tgtPorts[0] != 'None':

        for tgtScopeBegin in range(int(tgtRange)):
            t = Thread(target=connScanner, args=(tgtHost, int(tgtScopeBegin)))
            t.start()
        for tgtPort in tgtPorts:
            c = Thread(target=connScanner, args=(tgtHost, int(tgtPort)))
            c.start()
        print(int(tgtRange[1])-int(tgtRange[0]))
def main():
    parser = argparse.ArgumentParser(description='Scanner')
    parser.add_argument('-H','-host', help='Target host IP or URL')
    parser.add_argument('-p','-port', help='Targeted port/s, comma delimited')
    parser.add_argument('-r','-range', help='Targeted range of ports till the passed number(without it included)')
    args = parser.parse_args()
    tgtHost = args.H
    tgtPorts = str(args.p).split(',')
    tgtRange = str(args.r)
    if (tgtHost is None):
        parser.print_help()
        exit(0)
    timeStart = time.time()
    print(f'start scanning {tgtHost}')
    portScanner(tgtHost, tgtPorts, tgtRange)
    timeEnd = time.time()
    timeSpan = timeEnd - timeStart
    print(f'{tgtHost} scan Completed in {timeSpan}')

if __name__ == '__main__':
    tracemalloc.start()
    main()
exit(0)
