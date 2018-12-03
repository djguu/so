# API email sender

## Instrucoes de instalacao de email

1. Instalar Postfix

    * ```sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules```

   No caso se ainda nao ter instalado o Postfix ira ver uma pagina de setup em que apenas e necessario selecionar "Internet Site" e seguir em frente.

2. Editar o ficheiro principal

    * ```sudo -H gedit /etc/postfix/main.cf```

    Inserir estas linhas no final do ficheiro

    * ```
         relayhost = [smtp.gmail.com]:587
         smtp_sasl_auth_enable = yes
         smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
         smtp_sasl_security_options = noanonymous
         smtp_tls_CAfile = /etc/postfix/cacert.pem
         smtp_use_tls = yes
      ``` 

3. Editar o ficheiro de conta

    * ```sudo -H gedit /etc/postfix/sasl_passwd```

    Inserir estes dados

    * ```[smtp.gmail.com]:587    USERMAIL@gmail.com:PASSWORD```

4. Dar permissao e atualizar o ficheiro de configuracao

    * ```sudo chmod 400 /etc/postfix/sasl_passwd
         sudo postmap /etc/postfix/sasl_passwd
      ```

5. Validar certificados

    * ```cat /etc/ssl/certs/Thawte_Premium_Server_CA.pem | sudo tee -a /etc/postfix/cacert.pem```

    Nota: Caso este comando indique um erro seguir para o passo **Criar certificado**

6. Reiniciar o servidor de postfix

    * ```sudo /etc/init.d/postfix reload```


### Criar certificado
    (Apenas se deu erro no processo de validacao de certificado)

1. Criar o ficheiro Thawte

    * ```sudo touch /etc/ssl/certs/Thawte_Premium_Server_CA.pem```

2. Editar o ficheiro

    * ```sudo -H gedit /etc/ssl/certs/Thawte_Premium_Server_CA.pem```

    Inserir o seguinte texto

```
-----BEGIN CERTIFICATE-----
MIIDJzCCApCgAwIBAgIBATANBgkqhkiG9w0BAQQFADCBzjELMAkGA1UEBhMCWkExFTATBgNVBAgT
DFdlc3Rlcm4gQ2FwZTESMBAGA1UEBxMJQ2FwZSBUb3duMR0wGwYDVQQKExRUaGF3dGUgQ29uc3Vs
dGluZyBjYzEoMCYGA1UECxMfQ2VydGlmaWNhdGlvbiBTZXJ2aWNlcyBEaXZpc2lvbjEhMB8GA1UE
AxMYVGhhd3RlIFByZW1pdW0gU2VydmVyIENBMSgwJgYJKoZIhvcNAQkBFhlwcmVtaXVtLXNlcnZl
ckB0aGF3dGUuY29tMB4XDTk2MDgwMTAwMDAwMFoXDTIwMTIzMTIzNTk1OVowgc4xCzAJBgNVBAYT
AlpBMRUwEwYDVQQIEwxXZXN0ZXJuIENhcGUxEjAQBgNVBAcTCUNhcGUgVG93bjEdMBsGA1UEChMU
VGhhd3RlIENvbnN1bHRpbmcgY2MxKDAmBgNVBAsTH0NlcnRpZmljYXRpb24gU2VydmljZXMgRGl2
aXNpb24xITAfBgNVBAMTGFRoYXd0ZSBQcmVtaXVtIFNlcnZlciBDQTEoMCYGCSqGSIb3DQEJARYZ
cHJlbWl1bS1zZXJ2ZXJAdGhhd3RlLmNvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA0jY2
aovXwlue2oFBYo847kkEVdbQ7xwblRZH7xhINTpS9CtqBo87L+pW46+GjZ4X9560ZXUCTe/LCaIh
Udib0GfQug2SBhRz1JPLlyoAnFxODLz6FVL88kRu2hFKbgifLy3j+ao6hnO2RlNYyIkFvYMRuHM/
qgeN9EJN50CdHDcCAwEAAaMTMBEwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQQFAAOBgQAm
SCwWwlj66BZ0DKqqX1Q/8tfJeGBeXm43YyJ3Nn6yF8Q0ufUIhfzJATj/Tb7yFkJD57taRvvBxhEf
8UqwKEbJw8RCfbz6q1lu1bdRiBHjpIUZa4JMpAwSremkrj/xw0llmozFyD4lt5SZu5IycQfwhl7t
UCemDaYj+bvLpgcUQg==
-----END CERTIFICATE-----
```

3. Validar certificados

    * ```cat /etc/ssl/certs/Thawte_Premium_Server_CA.pem | sudo tee -a /etc/postfix/cacert.pem```

4. Reiniciar o servidor de postfix

    * ```sudo /etc/init.d/postfix reload```


### Testar envio de email

Para testar um envio de email basta este codigo

```echo "Test mail from postfix" | mail -s "Test Postfix" you@example.com```

Ou com um ficheiro

```mail -s "Test Postfix" you@example.com < body.txt```



## Intrucoes de Criacao de CronJob

1. Aceder ao ficheiro de configuracao

    ```crontab -e```

2. Adicionar a seguinte linha

    ```0 0 * * * /path/to/main_file```

    Esta linha sera responsavel pelo programa correr todos os dias a meia noite.

    Para modificar apenas tera que mudificar da seguinte forma

    ```
*     *     *     *     *  Command to be executed 
-     -     -     -     - 
|     |     |     |     | 
|     |     |     |     +----- Day of week (0-7) 
|     |     |     +------- Month (1 - 12) 
|     |     +--------- Day of month (1 - 31) 
|     +----------- Hour (0 - 23) 
+------------- Min (0 - 59) 
```

    Ao gravar se nao der erro o cronjob esta ativo
