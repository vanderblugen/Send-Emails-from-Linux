# send-email-from-linux
This information was obtained from https://medium.com/swlh/setting-up-gmail-and-other-email-on-a-raspberry-pi-6f7e3ad3d0e
This configuration uses Gmail as the email carrier.  Need to enable less secure email or enable 2 factor with app passwords for this configuration to work.

## Install needed apps
```shell
sudo apt install postfix libsasl2-modules mailutils -y
```

Select internet site on installation

## Add email password
```shell
sudo nano -B /etc/postfix/sasl/sasl_passwd
```

Add 'smtp.gmail.com:587 username@gmail.com:password' with the appropriate information
`Crtl+X' and `Enter` to save and exit

## Update the password file
```shell
sudo chmod u=rw,go= /etc/postfix/sasl/sasl_passwd
sudo postmap /etc/postfix/sasl/sasl_passwd
```

## Edit postfix configuration file
```shell
sudo cp /etc/postfix/main.cf !#$.dist
sudo nano /etc/postfix/main.cf
```

Change `relayhost = `
to `relayhost = smtp.gmail.com:587`

Change `smtp_tls_security_level=may` to `#smtp_tls_security_level=may`

Add this onto the bottom

```shell
# Enable authentication using SASL.
smtp_sasl_auth_enable = yes
# Use transport layer security (TLS) encryption.
smtp_tls_security_level = encrypt
# Do not allow anonymous authentication.
smtp_sasl_security_options = noanonymous
# Specify where to find the login information.
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
# Where to find the certificate authority (CA) certificates.
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

Exit and Save with `Ctrl+X` and `Enter`

## Send a test email

```shell
echo "Subject: Test Email Subject" | mail -a test_email@address.com < echo "Here is a test email"
```
