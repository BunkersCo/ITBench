# Renouvellement d'un certificat SSL pour Microsoft Exchange Server

### Exemple de procédure résultant à un succès

- Les étapes

	1 - Branchement au serveur EMS du client

	2 - Ouverture du Exchange management Shell

	3 - Nous allons valider le baille et la présence du certificat en cours avec

	Get-exchangeCertificate

	4 - Étudier le listing des certificats . Celui actif est contient la mention ip.ws (imap,pop,web,smtp).

	5 - Obtenir plus d'info sur le certificat ciblé:

	Get-exchangeCertificate -thumbprint | fl

	6 - Création d'une nouvelle demande de certificat en générant la clef en utilisant le BON CN et les domaines nécessaire pour l'installation:

	new-exchangecertificate -GenerateRequest -SubjectName "C=CA, S=QC, L=Haut-st-Laurent, O=mrchsl, OU=mrchsl, CN=remote.mrchsl.com" -DomainName remote.mrchsl.com, webmail.mrchsl.com, autodiscover.mrchsl.com, webmail.cldhsl.ca, autodiscover.cldhsl.ca -FriendlyName remote.mrchsl.com -PrivateKeyExportable $True

	7 - Sauvegardarder la demande CRT , ainsi que la clef dans un fichier texte situé dans la racine du système au nom de l'année (suivre l'arborescence présente)

	8 - Achat du certificat chez Godaddy

	9- Vérification du IIS manager , vérifier si le crochet du required SSL est actif: Expand pour voir les sites  (le décocher) (vérification des redirection http, si elles sont présente on doit retirer le crochet)

	10 Import-ExchangeCertificate -FileData ([Byte[]]$(Get-Content -Path C:\Certificat\Exchange2010\2018\new\eddef987b8b7b846.crt -Encoding byte -ReadCount 0))
     
	11- Get-ExchangeCertificate (pour avoir le thumprint du certificat)

	12- enable-exchangecertificate -thumbprint 12C57279F81634083F0251025344649E47D3B340 -services "IMAP, POP, IIS, SMTP"

	13- Remove-exchangecertificat 


###############################################################