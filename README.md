# actividad2estructura
f = open("C:\\Users\\306\\Downloads\\access.log", "r")
print(f.read())
from os import access
import requests
import re
respuesta = requests.get(access.log)


if respuesta.status_code == 200:
    logs = respuesta.text  

 
    patron_ip = r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"
    ips = re.findall(patron_ip, logs)

    
    patron_fecha_hora = r"\[(\d{2}/[A-Za-z]+/\d{4}):(\d{2}:\d{2}:\d{2}) \+\d{4}\]"
    fechas_horas = re.findall(patron_fecha_hora, logs)

    
    patron_get = r'"GET\s(\/[^\s]*)'
    gets = re.findall(patron_get, logs)

   
    if ips and fechas_horas and gets:
        print("Datos extraídos de los logs:\n")
        for ip, (fecha, hora), ruta in zip(ips, fechas_horas, gets):
            print(f'IP: {ip}, Fecha: {fecha}, Hora: {hora}, GET: {ruta}')
    else:
        print("No se encontraron direcciones IP, fechas, horas o solicitudes GET en los logs.")
else:
    print(f"Error al obtener los logs. Código de estado: {respuesta.status_code}")
