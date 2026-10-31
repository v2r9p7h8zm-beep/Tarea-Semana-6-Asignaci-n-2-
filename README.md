Práctica de Seguridad de Redes: AAA + RADIUS + Active Directory
Estudiante: Jennifer Lopez
Matrícula: 20240860
Dominio: itla.local
Entorno: GNS3, Windows Server 2022, VMware y Network Policy Server
(NPS)
Fecha de las evidencias: 30--31 de octubre de 2026
1. Descripción
Este repositorio reúne la documentación técnica y el material de
configuración de una práctica de laboratorio orientada a integrar
autenticación, autorización y accounting (AAA) en un router Cisco
mediante RADIUS, utilizando Windows Server 2022, Active Directory y
Network Policy Server (NPS).

La topología se trabajó con GNS3 en un equipo y Windows Server 2022 en
una máquina virtual VMware ejecutada en un segundo equipo. La
comunicación entre ambos dependía del puente de red hacia la interfaz
física y de que los equipos pudieran comunicarse dentro del segmento
configurado.
2. Contenido de la entrega
  Archivo                                                   Descripción
  Informe_AAA_RADIUS_RECONSTRUIDO_DESDE_CERO.docx         Informe técnico con arquitectura,
direccionamiento, configuración,
pruebas, incidencias y anexo de las
122 capturas originales.

  Anexo_Scripts_AAA_RADIUS_RECONSTRUIDO_DESDE_CERO.docx   Comandos IOS, VPCS, SSH,
AAA/RADIUS, verificaciones y
parámetros utilizados en Windows
Server/NPS.
README.md                                               Guía de lectura, alcance, topología
y advertencias de uso de la
entrega.
3. Arquitectura y direccionamiento
  Equipo/servicio             Dirección o parámetro
  Router R1                   192.168.10.1/24
PC1 (VPCS)                  192.168.10.10/24
Windows Server 2022 / NPS   192.168.10.100/24
Dominio Active Directory    itla.local
RADIUS Authentication       UDP 1812
RADIUS Accounting           UDP 1813

Componentes: R1 Cisco 7200 emulado en GNS3, switch Ethernet genérico
de GNS3, PC1/VPCS y Cloud1 para el enlace con la red física. Windows
Server 2022 se ejecutó en VMware en un segundo computador.
4. Resumen de configuración
Router R1
- Interfaz FastEthernet0/0 configurada con 192.168.10.1/24.
- AAA habilitado mediante aaa new-model.
- Servidor RADIUS NPS y grupo NPS-GROUP definidos.
- Listas de autenticación y autorización con respaldo local.
- Usuarios locales de respaldo con privilegios 15, 5 y 10.
- SSH versión 2 y controles de acceso a las líneas.
- Configuración de bloqueo de intentos y registro de eventos.
Windows Server 2022
- Dominio de Active Directory: itla.local.
- Grupos de seguridad: Level 0, Level 5 y Level 10.
- Servicio NPS preparado para recibir solicitudes RADIUS de R1.
- Cliente RADIUS: R1 (192.168.10.1).
- Políticas previstas: ITLA-LEVEL0, ITLA-LEVEL5 e ITLA-LEVEL10.
Atributos de privilegio Cisco
Los niveles se expresan mediante el atributo Cisco Cisco-AVPair
(Vendor ID 9), con estos valores:
- Level 0: shell:priv-lvl=15
- Level 5: shell:priv-lvl=5
- Level 10: shell:priv-lvl=10
En las evidencias revisadas quedó visible el atributo de Level 5. Los
valores de Level 0 y Level 10 se documentan como valores previstos y
deben verificarse individualmente antes de considerarlos probados.
5. Pruebas y estado de validación
Las capturas documentan la preparación de GNS3, la configuración de R1,
la instalación/configuración de Windows Server y el trabajo con Active
Directory y NPS.
- Se observó conectividad de PC1 hacia R1.
- La interfaz FastEthernet0/0 de R1 se observó up/up.
- show ip ssh confirmó SSH versión 2.
- En una prueba, R1 no recibió respuesta al ejecutar
  ping 192.168.10.100 (0/5).
Por ese resultado, la entrega no afirma que se haya demostrado una
autenticación RADIUS exitosa de extremo a extremo. Para validar esa
etapa se necesita conectividad IP entre R1 y NPS, una solicitud de
autenticación real y el evento correspondiente en los registros de NPS.
6. Limitaciones conocidas
1. El switch Ethernet genérico de GNS3 no es un switch Cisco IOS
   administrable. No permite demostrar AAA/RADIUS/SSH en un switch.
2. Windows Server se ejecutó en un segundo equipo, por lo que la
   conectividad depende del adaptador físico, el puente de VMware/GNS3
   y la configuración de la LAN.
3. En una captura de IPv4 se observa una puerta de enlace
   192.168.101.0, que no pertenece a la subred 192.168.10.0/24.
   Debe confirmarse/corregirse con el gateway real; no debe copiarse
   como valor válido.
4. IOS rechazó la variante de accounting de comandos con el grupo
   RADIUS. La función de accounting de comandos requiere TACACS+ en la
   plataforma utilizada.
5. El método de respaldo local puede permitir el acceso aunque NPS no
   esté disponible. Por eso, un inicio de sesión exitoso por sí solo no
   prueba que RADIUS haya autenticado al usuario.
7. Cómo revisar la entrega
1. Leer primero el Informe técnico para comprender la topología, el
   proceso, las decisiones y los resultados.
2. Consultar el Anexo de scripts para localizar los comandos y
   parámetros de cada etapa.
3. Revisar las 122 capturas del informe en orden para relacionar cada
   evidencia con la fase correspondiente.
4. Distinguir siempre entre comandos ejecutados, parámetros
   configurados mediante asistentes gráficos y pruebas que quedaron
   pendientes de validación.
8. Seguridad
- No publicar contraseñas de usuarios, hashes ni secretos compartidos
  RADIUS.
- Los valores sensibles deben configurarse localmente y coincidir en
  R1 y NPS.
- No ejecutar bloques completos sin revisar el modo IOS, la interfaz y
  el direccionamiento.
- Desactivar los comandos debug al terminar las pruebas con
  undebug all.
- Guardar los cambios del router solo después de verificar la
  configuración.
9. Nota de reproducibilidad
Los comandos del anexo sirven como referencia para reproducir las
configuraciones documentadas. Las operaciones de Windows Server y NPS se
realizaron principalmente mediante asistentes gráficos, por lo que el
anexo describe sus parámetros y pasos funcionales, no un script
automatizado único. Los valores marcados como previstos o de
comprobación no deben presentarse como evidencia de ejecución
satisfactoria.
