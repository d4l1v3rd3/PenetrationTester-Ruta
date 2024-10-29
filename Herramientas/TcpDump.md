# TcpDump

Es una herramienta que utiliza la libreria "libpcap" escrita en C y C++ basado en los sistemas Unix, bastante estable y con velocidad optima, suportea "winpcap"

- Capturaremos paquetes y guardaremos el archivo
- Pondremos filtros a los paquetes
- Controlares como los paquetes se leen

# CAPTURA BÁSICA DE PAQUETE 

Podemos ejecutar "tcpdump" sin la necesitdad de ningun argumento o especificando primero de todo necesitaremos estar en escucha 

```
-i INTERFACE
```

Para elegir todas las interfaces disponibles

```
-i any
```

Para ver la interfaz nuestra

```
ip a s
```

## GUARDAR PAQUETES

En muchos casos habrá muchos paquetes. Podemos guardar el archivo usando

```
-w FILE
```

Normalmente la extensión es ".pcap"

## LERR PAQUETES

Usaremos RcpDump para leer paquetes

```
-r FILE
```


## LIMITAR EL NUMERO CAPTURADO DE PAQUETES

Especificaremos el numero de paquetes que queremos capturar
```
-c COUNT
```

## NO RESOLVER LA IP Y LOS PUERTOS

```
-n
-nn
```

EJEMPLO:

```
sudo tcpdump -i ens5 -c 5 -n
```

## VERBOSE

```
-v
```

# FILTROS

```
sudo tcpdump host nodo313.net -w http.pcap
```

## FILTRAR PUERTO

```
sudo tcpdump -i ens5 port 53 -n
```

## FILTRAR POR PROTOCOLO

```
sudo tcpdump -i ens5 icmp -n
```

```
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc
```



