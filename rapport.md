# TP 2 - Bash

## Exercice 1. Variables d’environnement

1) Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?
`Le script PATH indique ou trouver les commandes tapées par l'utilisateur`
</br>

2) Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans
votre répertoire personnel ?
`Home pour la variable d'environement contenant le chemin du répertoire personnel.`
</br>
3) Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.
```
* LANG : la variable d’environnement LANG détermine la langue que les logiciels
utilisent pour communiquer avec l’utilisateur
* PWD : elle permet de retourner le nom de répertoire courant
* OLDPWD : elle permet de retourner l'ancien répertoire courant
* SHELL : elle indique l'interpréteur shell utilisé par défaut. La valeur habituelle sous linux est /bin/bash 
* "_" : elle retourne la poubelle
```
</br>

4) Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.

```
* MY_VAR=toto
* printenv MY_VAR
* echo $MY_VAR
```

5) Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.
```
la commande bash ouvre un autre bash et vu que la création d'une variable locale n'est disponible que dans la session courante,
on n'y a pas accès, pour cela, il faut faire une variable d'environement.
Si on veut garder une variable après avoir eteint la machine, il faut l'ajouter dans ~/.bashrc
```

6) Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.
```
unset MY_VAR
export MY_VAR=toto
Comme dit précédement, avec une variable d'environement, on peut avoir accès à ces variables, même dans un autre bash
```
7) Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace.
Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.
```export NOMS="RISS GOUY"
echo $NOMS
```
8) Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2
sont vos deux noms) en utilisant la variable NOMS.
*echo "Bonjour à vous deux, $NOMS"*

9) Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande
unset ?
```
Ne rien affecter à une variable, crée la variable avec rien dedans
Alors que "unset" supprime la variable, pour plus d'information unset --help*
```

10) Utilisez la commande echo pour écrire exactement la phrase *:$HOME=*
chemin (où chemin est votredossier personnel d’après bash)
*echo '$HOME =' $HOME*

## Programmation Bash
```
PATH_SCRIPT=`pwd`
export PATH=$PATH:$PATH_SCRIPT
```

## Exercice 2. Contrôle de mot de passe
Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au
contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par
l’utilisateur ne doit pas s’afficher.
```
#!/bin/bash
#name testpwd.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1
USER_PASSWORD="pwdtest"
read -s -p 'Veuillez saisir votre mot de passe' password
if [ $password = $USER_PASSWORD ]
then
    echo "password OK"
else
    echo "password is not OK"
fi
```

## Exercice 3. Expressions rationnelles
Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre
est un nombre réel :

```
#!/bin/bash
#name isnumber.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1
function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]] ; then
        return 1
    else
        return 0
    fi
}
is_number $1
if [[ $? = "0" ]]
then 
    echo "argument pass is a number"
else
    echo "argument pass is not a number"
fi
```

## Exercice 4. Contrôle d’utilisateur
Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le
script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”,
où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre
script, le message doit changer automatiquement)

```
#!/bin/bash
#name ctrl_user.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1

if [[ $1 != "" ]]
then
    user=`cut -d: -f1 /etc/passwd | grep ^$1$`

    if [[ $user == "" ]]
    then
        echo 'utilisateur introuvable
    else
        echo 'utilisateur trouvé'
    fi
else
    echo 'Utilisation : bash Nom_du_script Nom_utilisateur'
fi
```

## Exercice 5. Factorielle
Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que
l’utilisateur saisit toujours un entier naturel).
```
#!/bin/bash
#name factorial.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1
function factorial(){
    if [[ $1 -le 1 ]]
    then
        echo 1
    else
        last=$(factorial $(( $1 - 1)))
        echo $(( $1 * last ))
    fi
}

factorial $1
```


## Exercice 6. Le juste prix
Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner.
Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).

```
#!/bin/bash
#name juste_prix.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1

number=$((($RANDOM % 100) + 1))

function juste_prix(){

    echo "Bienvenue sur le jeu du juste prix, un nombre aléatoire vient d'être généré'
    read -p 'Veuillez saisir un nombre compris entre 1 et 100 ' user_number
    while [ $user_number -ne $number]
    do
        if [ $user_number -lt $number ]
        then
            echo -e "Votre nombre saisi est trop petit\n"
        else
            echo -e "Votre nombre saisi est trop grand\n"
        fi
        read -p "Veuillez saisir un nombre compris entre 1 et 100 ' user_number
    done
    echo "Bravo, vous avez trouvé le nombre : $number"
}

juste_prix
```



## Exercice 7. Statistiques
1) Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max
et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres
sont bien des entiers.
2) Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)
3) Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et
stockées au fur et à mesure dans un tableau.

```
#!/bin/bash
#name stats.sh
#date 19/02/2020
#propriétaire RISS GOUY
#version 1


function usage(){
    echo "Usage $0 waiting numbers parameters between -100 and 100"

}

function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]] ; then
        return 1
    else
        return 0
    fi
}

function doStat()
{
    read -p "Veuillez saisir une note ou appuyer sur entrer" note
    while ! [[ $note = '' ]]
    do
        is_number $note
        if [ $? = "1" ]
        then
            usage
        else
            if [ $note -lt -100 ] || [ $note -gt 100]
                usage
            else
                echo $note >> fileNote.txt
                echo "note ajoutée"
            fi
        fi
        read -p "Veuillez saisir une note ou appuyer sur entrer" note
    done
    val=`cat fileNote.txt`
    i=1
    while read line
    do
        tab[$i]=$line
        i=$(($i+1))
    done < val
    IFS=$'\n' min=($(sort <<< "$t{tab[*]}"));
    unset IFS
    IFS=$'\n' max=($(sort -r <<< "$t{tab[*]}"));
    unset IFS
    moyenne=0
    for param in ${tab[@]}
    do
        moyenne=$(($moyenne+$param))
    done
    moyenne=$(($moyenne/${#tab[@]}))
    echo "la liste des notes : ${tab[@]}"
    echo "la valeur minimum est de : $min"
    echo "la valeur maximale est de : $max"
    echo "la moyenne est égale à : $moyenne"


}

doStat