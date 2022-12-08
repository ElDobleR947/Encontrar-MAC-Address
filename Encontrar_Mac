from netmiko import ConnectHandler
import re
#ingresamos la IP del switch core y preguntamos por la MAC
username = input("Ingresa la ip del switch en el que estamos: ")
preguntar_MAC = input("ingrese la mac buscada: ")
ip = username
#generamos un bucle para que se pueda meter a los distintos switches 
while True:
    #hacemos la conexion por ssh
    switch = {
        'device_type': "cisco_ios_ssh",
        'ip': ip,
        'username': "cisco",
        'password': "cisco",}
    Conectar = ConnectHandler(**switch)
    #tiramos el comando para ver la tabla de mac address
    tirarcomado = Conectar.send_command("show mac address-table | in " + preguntar_MAC)
    print("1", tirarcomado)
    #con la ayuda de algunas expresiones regulares sacamos algunos datos como la interfaz, la mac address y la IP
    interfaz_expresionreg = re.compile(r'\w\w\w?\d\/\d/?\d?\d?')
    buscar_interfaz = interfaz_expresionreg.search(tirarcomado)
    interfaz = buscar_interfaz.group()
    #expresion regular para sacar la MAC
    MAC_expresionreg = re.compile(r"[a-f0-9]{4}..........")
    buscar_MAC = MAC_expresionreg.search(tirarcomado)
    print("2",buscar_MAC)
    MAC_ADDRESS = buscar_MAC.group()
    # creamos una condicion para si las dos mac address coinciden tire el comando show cdp neighbors detail de la interfaz
    if MAC_ADDRESS == preguntar_MAC:
        output2 = Conectar.send_command("show cdp neighbors " + interfaz + " detail")
        #con otra expresion regular sacamos lo que es la IP
        IP_expresionreg = re.compile(r'(\d+\.+\d+\.\d+\.\d+)')
        IP = IP_expresionreg.search(output2)
        #con un bloque try generamos a proposito un error para que trate de buscar dentro de una computadora
        try: 
            ip = IP.group()
        #como se genera el error cae en el except y se ejecuta lo que esta dentro del bloque
        except:
            print("Se encontro la mac!!")
            print("En el puerto: ", interfaz)
            switch = (Conectar.send_command("sh running-config | include hostname"))
            print("En el switch", switch)
            break
    else:
        print("MAC no encontrada")
        break
