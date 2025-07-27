import psutil
import time
from datetime import datetime

svartlistade_ips = {"192.241.135.45", "185.199.108.153"}
loggfil = "nätverks_logg.txt"

def logga(ip):
    with open(loggfil, "a") as f:
        f.write(f"{datetime.now()} - Upptäckt svartlistad IP: {ip}\n")

def monitor():
    # Print 10x 
    for _ in range(10):
        print("Övervakar svartlistade IP-adresser... Tryck")
        time.sleep(0.2)

    print("Startar övervakning... Tryck CTRL+C för att stoppa")

    try:
        while True:
            anslutningar = psutil.net_connections(kind='inet')
            for x in anslutningar:
                if x.raddr:
                    ip = x.raddr.ip
                    if ip in svartlistade_ips:
                        print(f"[!] Upptäckt anslutning till svartlistad IP: {ip}")
                        logga(ip)
            time.sleep(1)   
    except KeyboardInterrupt:
        print("\n[!] Avslutar övervakning...")

if __name__ == "__main__":
    monitor()
