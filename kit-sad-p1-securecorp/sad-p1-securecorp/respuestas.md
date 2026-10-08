# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos: Ismael Romero Romero**
**Usuario: iromero**

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: iromero
- mtorres:

---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?
* El issuer es ES y es válido hasta 3650 días.
* Porque CA es autoridad máxima es firma su propio certificado y ldap.crt lo autoriza CA.

**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh: ldapsearch -x -LLL \
  -H ldap://ldap.securecorp.local \
  -b "cn=rrhh,ou=groups,dc=securecorp,dc=local" \
  "(objectClass=*)" member
respuesta: member: uid=mtorres,ou=people,dc=securecorp,dc=local
member: uid=lromero,ou=people,dc=securecorp,dc=local


b) cn y mail de todas las personas: ldapsearch -x -LLL \
  -H ldap://ldap.securecorp.local \
  -b "ou=people,dc=securecorp,dc=local" \
  "(objectClass=inetOrgPerson)" \
  cn mail
respuesta: dn: uid=lromero,ou=people,dc=securecorp,dc=local
cn: Lucia Romero
mail: lromero@securecorp.local

dn: uid=mtorres,ou=people,dc=securecorp,dc=local
cn: Marta Torres
mail: mtorres@securecorp.local

dn: uid=iromero,ou=people,dc=securecorp,dc=local
cn: Ismael Romero
mail: iromero@securecorp.local


```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?
Porque si no no tiene permiso para leerlo y el servicio TLS fallará. 
Tiene permisos 600 para que nadie lea el archivo y suplante la identidad del servidor.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?
"ldaps:/// ldapi:///"
Porque ldaps tiene el puerto 636 y ldap tiene 389 y ese había que deshabilitarlo. 

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?
Porque tenía que añadir la línea que proporciona el certificado ya que el cliente no conocía el CA.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

```Ticket cache: FILE:/tmp/krb5cc_0
Default principal: iromero@SECURECORP.LOCAL

Valid starting     Expires            Service principal
10/08/26 18:03:10  10/09/26 04:03:10  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL
	renew until 10/15/26 18:03:10
10/08/26 18:05:33  10/09/26 04:03:10  host/web.securecorp.local@SECURECORP.LOCAL
	renew until 10/15/26 18:03:10


```

**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?

build contruye mediante una imagen local y image descarga la imagen de internet. 8081 es el puerto de mi host y escucha al 80 .
**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?
en web puse la información en el dockerfile, y en cliente se hizo porque a estaba en ejecución. Si hago reset perdería el docker de web y todo lo que he configurado en servidor y cliente.
