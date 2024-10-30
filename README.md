# Decoupage-de-sous-reseaux

## Challenge  

>### Informations :  

- Réseau : 172.16.1.0 /24
  
- Découpage symétrique et asymétrique
  
- Pôles :  
    - Informatique : 50 équipements  
    - Développement : 12 équipements  
    - Administratif : 20 éuqipements  
    - Technicien : 15 équipements  
          -> 4 sous-réseaux    
  
  
>### I - Découpage symétrique

Nous prenons le plus grand nombre d'équipements nécessaire, ici, c'est 50 équipements pour le pôle informatique.  
On va faire un découpage symétrique, de ce fait, les 4 sous-réseaux auront la même plage d'adresses IP.  
On se réfère au tableau des puissances de 2 pour détérminer le nombre d'hôtes possible d'affecter à chaque sous-réseau.  
Le nombre supérieur à 50 est 64, et la puissance de 2 qui est associée à 64 est 6.  
Chaque sous-réseaux pourra donc être constitué de 64 adresses IP différentes, mais on ne pourra pas adresser les 64 à chaque machine.  
Il faut soustraire l'_IP du réseau_ et l'_IP du broadcast_, donc 2 adresses. Ce qui fait : 64 - 2 = 62.  
Il y aura donc 62 adresses IP possibles d'affecter à des _hôtes_ sur ce réseau.  

**Calcul du masque de sous-réseau :**  
On prend la puissance de 2 associée à 64 dans le tableau des puissances de 2 : 6.
Un masque est constitué de 32bits. Donc 32bits - 6bits = un masque à 26bits pour les 4 sous-réseaux

| Pôle          | Adresse du réseau| Début de plage | Fin de plage | Adresse de broadcast |
|---------------|------------------|--------------- |--------------|----------------------|
| Informatique  |  172.16.1.0 /26  |  172.16.1.1    | 172.16.1.62  |     172.16.1.63      |
| Développement |  172.16.1.64 /26 |  172.16.1.65   | 172.16.1.126 |     172.16.1.127     |
| Administratif |  172.16.1.128 /26|  172.16.1.129  | 172.16.1.190 |     172.16.1.191     |
| Technicien    |  172.16.1.192 /26|  172.16.1.193  | 172.16.1.254 |     172.16.1.255     |  


>### II - Découpage asymétrique  

Dans un premier temps on calcule le nombre d'hôtes maximum pour chaque département :    
- Pôle informatique : 50 équipements. On se réfère au tableau des puissances de 2, la valeur au dessus est 64. Déduction adresse réseau et broadcast 64 - 2 = 62 adresses hôtes disponibles. Calcul du masque : 32 - 6 = /26
- Pôle développement : 12 équipements. Valeur au dessus = 16. 16 - 2 = 14. 32 - 4 = /28
- Pôle administratif : 20 équipements. Valeur au dessus = 32. 32 - 2 = 30. 32 - 5 = 27
- Pôle technicien : 15 équipements. Valeur au dessus = 16, mais ici on prendra 32 car en prenant 16 on n'aura pas assez d'adresses pour l'adresse réseau et l'adresse broadcast. 32 - 2 = 30. 32 - 5 = /27

Avec le découpage asymétrique, on commence par le sous-réseau qui va utiliser le plus d'équipements.

| Pôle          | Adresse du réseau| Début de plage | Fin de plage| Adresse de broadcast |
|---------------|------------------|----------------|-------------|----------------------|
| Informatique  |  172.16.1.0 /26  |  172.16.1.1    | 172.16.1.62 |     172.16.1.63      |
| Administratif |  172.16.1.64 /27 |  172.16.1.65   | 172.16.1.94 |     172.16.1.95      |
| Technicien    |  172.16.1.96 /27 |  172.16.1.97   | 172.16.1.126|     172.16.1.127     |
| Développement |  172.16.1.128 /28|  172.16.1.129  | 172.16.1.142|     172.16.1.143     |  

