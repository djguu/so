# API email sender

### Instrucoes de instalacao de email

1. Instalar Postfix

    * ```sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules```

   No caso se ainda nao ter instalado o Postfix ira ver uma pagina de setup em que apenas e necessario selecionar "Internet Site" e seguir em frente.

2. Editar o ficheiro original

    * ```sudo -H gedit /etc/postfix/main.cf```

    Inserir estas linhas no ficheiro no final

    * ```relayhost = [smtp.gmail.com]:587
         smtp_sasl_auth_enable = yes
         smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
         smtp_sasl_security_options = noanonymous
         smtp_tls_CAfile = /etc/postfix/cacert.pem
         smtp_use_tls = yes
    ```