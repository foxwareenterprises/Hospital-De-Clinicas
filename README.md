 S.I.G.S.M. - Sistema de Gestión de Traslados de Pacientes

Proyecto Final
Programación Full Stack 
Equipo FoxWare -Polo MVD 3°MA

Sistema interno desarrollado para el Hospital de Clínicas que gestiona los traslados de pacientes dentro del circuito nacional, la documentación clínica asociada y las encuestas de satisfacción, cumpliendo con estándares de accesibilidad (WCAG 2.1 AA) y seguridad institucional.



 Índice

- Descripción
- Módulos
- Tecnologías
- Requisitos
- Instalación
- Uso
- Estructura del Proyecto
- Equipo
- Documentación
- Licencia



 Descripción

El S.I.G.S.M. reemplaza el sistema de registros en papel utilizado actualmente por el Hospital de Clínicas, digitalizando los procesos de:
-Solicitud y asignación de traslados de pacientes.
-Carga y consulta de documentación clínica.
-Gestión de recursos (vehículos, conductores, enfermeros).
-Encuestas anónimas de satisfacción accesibles vía QR.

El sistema opera en intranet hospitalaria, con acceso restringido por roles y credenciales institucionales.



Módulos
1. Módulo Documentación
-Panel administrativo para carga de documentos clínicos.
-Visualización mobile-first accesible mediante código QR.
-Formulario de encuesta de satisfacción anónima.

2. Módulo Ambulancias (No existente)
-Panel de gestión de traslados.
-Formulario de alta de traslado con validación de campos clínicos.
-Vista de seguimiento de estado en tiempo real.

 Tecnologías
Backend- PHP 8.1+ 
-Base de Datos- MySQL 8.0 / MariaDB 10.6+ 
-Frontend- HTML5, CSS3, Bootstrap 5.3 
-Servidor Web- Apache 2.4+ (XAMPP) 
-Control de Versiones- Git + GitHub 
-IDE- Visual Studio Code 



Requisitos
Antes de comenzar, asegúrate de tener instalados:

-XAMPP 8.2+ https://www.apachefriends.org/
-Git https://git-scm.com/
-Visual Studio Code https://code.visualstudio.com/
-Navegador moderno Chrome, Firefox o Edge



 Instalación

 1. Clonar el repositorio
'''bash
cd C:\xampp\htdocs
git clone https://github.com/ (nombre repositorio)
cd sigsm
