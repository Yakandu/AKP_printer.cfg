## APUNTES SOBRE KLIPPER

# Como bufar la máquina:

> Instalacion de klipper
https://github.com/KevinOConnor/klipper/blob/master/docs/Installation.md

> Plugin klipper para octoprint
https://github.com/mmone/OctoprintKlipperPlugin

> Configuracion base para AKPlus + tmc2130
https://github.com/correos/AKP_printer.cfg/blob/master/printer.cfg


Para drivers stock A4988:
https://github.com/correos/AKP_printer.cfg/blob/master/printer%20-%20stock.cfg

Revisar para cualquier consulta de modificaciones.
https://github.com/KevinOConnor/klipper/tree/master/config

>Una vez introducida la config con el plugin de octoprint pasar a realizar los checks:
https://github.com/KevinOConnor/klipper/blob/master/docs/Config_checks.md

> Ver como vuela la kossel




# Algunos comandos

HELP - Socorro, te saca todos los comandos.

STATUS - Check de la maquina

RESTART - menu

FIRMWARE_RESTART -

PROBE - Hasta romperte la cama

QUERY_PROBE - Muestra el estado de Z probe

QUERY_ENDSTOPS - M119

DUMP_TMC STEPPER=stepper_a/b/c - Estado del driver tmc

*por configurar:

BED_MESH_CALIBRATE

BED_MESH_OUTPUT

BED_MESH_OUTPUT

BED_MESH_CLEAR




### PINES

^ pin con pullup

! inversor de logica 

Format is: [^] [!] [chip_name:]




### Comandos de octoprint desde el LCD

Copiamos el menu.cfg del repositorio en /home/pi/klipper/klippy/extras/display/menu.cfg

Instalamos en octoprint el plugin "Action Commands" y creamos los siguientes comandos en el:

![actioncommands](https://github.com/correos/AKP_printer.cfg/blob/master/img/octoprint%20commands.PNG)
Reiniciamos el servicio Klipper o directamente la raspberry.

![actioncommandslcd](https://github.com/correos/AKP_printer.cfg/blob/master/img/comandoslcd.jpg)




### Pitido al finalizar la impresión

Añadimos en octoprint, gcode script/after print job completes:

>SET_PIN PIN=beeper VALUE=1
>G4 P1000 ; wait 1 s
>SET_PIN PIN=beeper VALUE=0

![beeper](https://github.com/maketo3D/AKP_printer.cfg/blob/master/img/beeperscript.PNG)




## CALIBRACIÓN

###   Comprobar Z probe

Lanzar QUERY_PROBE, si aparece TRIGGERED sin estar pulsado invertir el pin:

[probe]

pin: ^ar18  // normal

pin: ^!ar18  // invertido

z_offset: xx.xx // ajustamos tambien nuestro offset del sensor


###   DELTA_CALIBRATE

Tras la calibracion tendremos esta salida por terminal, tenemos que editar el printer.cfg con dichos valores y ejecutar RESTART.

Recv: // stepper_a: position_endstop: xxx.xxxxxx angle: xxx.xxxxxx

Recv: // stepper_b: position_endstop: xxx.xxxxxx angle: xxx.xxxxxx

Recv: // stepper_c: position_endstop: xxx.xxxxxx angle: xxx.xxxxxx

Recv: // delta_radius: xxx.xxxxxx

###  PID_CALIBRATE HEATER=extruder/heater_bed TARGET=Temperatura 
  
Tras terminar el ajuste PID, hay que introducir los valores nuevos en el printer.cfg y ejecutar RESTART.

Recv: // PID parameters: pid_Kp=xx.xxx pid_Ki=x.xxx pid_Kd=xxx.xxx


Si añadimos [WRITE_FILE=1] se envia un log a /tmp/heattest.txt

