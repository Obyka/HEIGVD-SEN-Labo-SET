# Rapport labo Introduction à The Social-Engineer Toolkit (SET) 
Auteurs : Jérôme Bagnoud, Florian Polier

## Exercice 1 - Credential Harvesting

La première étape consiste à choisir le bon module. Voici la séquence à choisir :

1) Social-Engineering Attacks
2) Website Attack Vectors
3) Credential Harvester Attack Method
4) Site Cloner

Voici nos paramètres 
- le site malicieux est hébergé en local
- la page que nous clonons est la page de login de github

![params](img\param_clone.jfif)

Et voici la comparaison du site originel et du clône

![compare](img\compare_pages.jfif)

Lorsque quelqu'un accède à la page malicieuse et tente de se connecter, nous le voyons dans l'outil.(ici, le login `myusername` et le mot de passe `mypassword` ont été récupérés)

![compare](img\stolen_passwords.jfif)

Et voici le même constat pour twitter

![compare](img\stolen_passwords_twitter.jfif)

## Exercice 2 - Créer une attaque de phishing

Dans cette étape, nous allons créer un mail de spearphishing avec un payload spécifique à l'intérieur d'un fichier PDF. Pour envoyer ce mail, nous allons utiliser le serveur SMTP de la HEIG-VD, moins regardant sur la dangerosité d'un mail que GMAIL par exemple.

Voici les différentes étapes afin de créer et envoyer l'attaque :

1) Spear-Phishing Attack Vectors
2) Perform a Mass Email Attack
3) Foxit PDF Reader v4.1.1 Title Stack Buffer Overflow
4) Windows Reverse TCP Shell
5) On laisse l'IP et le port du reverse shell par défaut
6) E-Mail Attack Single Email Address
7) Keep the filename, I don't care.
8) E-Mail Attack Single Email Address
9) One-Time Use Email Template
10) Entrez le sujet de l'e-mail ainsi que le corps de celui-ci
11) Entrez l'e-mail de destination
12) Entrez l'e-mail source
13) Entrez le nom que la victime verra sur l'email
14) Laissez le nom d'utilisateur et le mot de passe vide pour le serveur SMTP
15) Laissez le port par défaut
16) Mettez le message en non prioritaire
17) Mettez non pour l'utilisation de TLS
18) Mettez non pour l'utilisation du listener, on en aura pas besoin ici

Et voici le mail reçu avec la pièce jointe infectée.

![compare](img\mail_phishing.PNG)

Notre payload est conçu pour déclencher un buffer overflow dans la version 4.1.1 du logiciel Foxit PDF Reader. Une fois le fichier ouvert, la victime ouvre une connexion vers l'attaquant (reverse_tcp) et ce dernier obtient un shell via le handler de meterpreter, la cible est ainsi compromise.

### Exercice 3 - Explorer les liens "Phishy" et le courrier électronique "Phishy"

#### Analyse d'en-tête d'un mail de phishing

Pour cette analyse, nous avons choisi d'inspecter les en-têtes d'un mail de spam qui a été trouvé dans une de nos boîtes mails

Voici les en-têtes
```
Delivered-To: polier.florian@gmail.com
Received: by 2002:a67:e08f:0:0:0:0:0 with SMTP id f15csp1541677vsl;
        Thu, 12 Mar 2020 21:44:36 -0700 (PDT)
X-Google-Smtp-Source: ADFU+vsK+iCRmHTU35q8ovc4FKDvN/PhVMyzKlvtk6qRLoXK9GUt/xmOm8eaJn03a5/jZgQIy290
X-Received: by 2002:ad4:4502:: with SMTP id k2mr10737329qvu.85.1584074676707;
ARC-Authentication-Results: i=1; mx.google.com;
       spf=softfail (google.com: domain of transitioning nepasrepondre@e-i.com does not designate 201.20.46.94 as permitted sender) smtp.mailfrom=nepasrepondre@e-i.com
Return-Path: <nepasrepondre@e-i.com>
Received: from openzcs04.upmail.net.br (openzcs04.upmail.net.br. [201.20.46.94])
        by mx.google.com with ESMTPS id g25si4096035qtv.296.2020.03.12.21.44.35
        for <polier.florian@gmail.com>
        (version=TLS1_2 cipher=ECDHE-RSA-AES128-GCM-SHA256 bits=128/128);
        Thu, 12 Mar 2020 21:44:36 -0700 (PDT)
Received-SPF: softfail (google.com: domain of transitioning nepasrepondre@e-i.com does not designate 201.20.46.94 as permitted sender) client-ip=201.20.46.94;
Authentication-Results: mx.google.com;
       spf=softfail (google.com: domain of transitioning nepasrepondre@e-i.com does not designate 201.20.46.94 as permitted sender) smtp.mailfrom=nepasrepondre@e-i.com
Received: from openzcs04.upmail.net.br (localhost [127.0.0.1]) by openzcs04.upmail.net.br (Postfix) with ESMTPS id 317B646AEBB2 for <polier.florian@gmail.com>; Fri, 13 Mar 2020 01:44:34 -0300 (-03)
Received: from openzcs04.upmail.net.br (localhost [127.0.0.1]) by openzcs04.upmail.net.br (Postfix) with ESMTPS id 233CA46AEBB3 for <polier.florian@gmail.com>; Fri, 13 Mar 2020 01:44:34 -0300 (-03)
Received: from WIN-QR3QTCQGM3C (openzcs02.upmail.net.br [201.20.46.92]) by openzcs04.upmail.net.br (Postfix) with ESMTPSA id CA72546AEBB2 for <polier.florian@gmail.com>; Fri, 13 Mar 2020 01:44:33 -0300 (-03)
MIME-Version: 1.0
From: SWISSBANKERS <nepasrepondre@e-i.com>
To: polier.florian@gmail.com
Date: 13 Mar 2020 06:44:34 +0200
Subject: Verification Necessaire
Content-Type: multipart/related; type="text/html"; boundary=--boundary_224219_7cfddca0-06ec-4d92-a165-bb470351692a
Message-Id: <20200313044433.CA72546AEBB2@openzcs04.upmail.net.br>

----boundary_224219_7cfddca0-06ec-4d92-a165-bb470351692a
Content-Type: text/html; charset=utf-8
Content-Transfer-Encoding: base64

DQo8Qk9EWT48UD48QSBocmVmPSJodHRwczovL2Flcm9zb2x1dGlvbnMuY2giPjxJTUcgYm9yZGVy
PTAgaHNwYWNlPTAgYWx0PSIiIHNyYz0iY2lkOnBpYzEiIGFsaWduPWJhc2VsaW5lPjwvQT48L1A+
PC9CT0RZPg==
----boundary_224219_7cfddca0-06ec-4d92-a165-bb470351692a
Content-Type: image/jpg
Content-Transfer-Encoding: base64
Content-ID: <pic1>


----boundary_224219_7cfddca0-06ec-4d92-a165-bb470351692a--
```

Tout d'abord, nous pouvons détecter que le mail n'est pas légitime en comparant l'adresse du From: `nepasrepondre@e-i.com` avec les différents serveurs par lesquels le mail a été routé (e.g `openzcs04.upmail.net.br`)

De plus, la SPF n'est pas valide (en-tête Received-SPF), cela veut dire que le domaine dont provient le mail, n'autorise pas le serveur SMTP mentionné à utiliser son nom. La SPF est une entrée DNS, ou le serveur SMTP aurait dû apparaître. On peut le contrôler manuellement, en faisant la requête suivante.

![spf](img\spf.PNG)

Nous voyons dans l'entrée TXT commençant par v=spf1 que l'IP du serveur smtp n'est pas mentionnée.

En faisant un whois, nous pouvons nous intéresser à l'adresse d'où vient le mail

![spf](img\whois.PNG)

Comme nous le voyons, cette dernière vient du Brésil, ce qui finit par confirmer que c'est un mail frauduleux, puisque le mail d'origine devait venir d'une banque Suisse. 






