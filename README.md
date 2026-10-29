Informe de Auditoría de Red Wi-Fi Insegura

Módulo: Seguridad en el Aire y Criptografía Aplicada

1. Introducción

Este informe documenta los riesgos inherentes al tráfico de datos dentro de una infraestructura de red Wi-Fi pública sin cifrado perimetral. En estos entornos, el canal de comunicación ("aire") es compartido por múltiples estaciones de trabajo, lo que permite a un atacante potencial realizar actividades de intercepción de datos (sniffing), ataques de intermediario (Man-in-the-Middle) o el despliegue de redes gemelas malignas (Evil Twins). El objetivo de esta auditoría es contrastar el comportamiento de las solicitudes bajo protocolos no seguros frente a esquemas de protección basados en criptografía corporativa.

2. Sitio Analizado

• Host Visitado: http://ejemplo-inseguro.com
• Protocolo de Red: HTTP (Hypertext Transfer Protocol) / Versión 1.1 o 2 sin TLS.
• Herramienta de Diagnóstico: Pestaña Network (Red) de las Herramientas de Desarrollador (F12).

3. Evidencia Observada (Análisis de Tráfico)

Tras realizar la captura de la primera solicitud en la pestaña Network, se identificaron en texto plano los siguientes elementos técnicos fundamentales dentro de las cabeceras (headers):
• Host: ejemplo-inseguro.com (Expone directamente el dominio de destino al que el usuario intenta conectarse).
• Método HTTP: GET (Indica la acción operativa de solicitud de recursos hacia el servidor web).
• URL Solicitada: http://ejemplo-inseguro.com/ (Dirección estructurada completa desprovista de esquemas de seguridad).
• User-Agent: Revela de forma precisa la versión del sistema operativo, el navegador y la arquitectura del dispositivo del cliente (facilitando técnicas de Fingerprinting).
• Headers de Conexión: Parámetros como Accept-Language y Upgrade-Insecure-Requests que viajan sin ningún tipo de codificación defensiva.

4. Riesgos Encontrados (Navegación HTTP)

Navegar mediante el protocolo HTTP clásico en una Wi-Fi pública genera una exposición crítica debido a las siguientes vulnerabilidades:
• Lectura en Texto Plano: El tráfico HTTP carece de mecanismos de confidencialidad. Cualquier paquete capturado mediante herramientas de inspección como Wireshark equivale a una postal abierta; un atacante puede leer credenciales, cookies de sesión, correos electrónicos, datos bancarios y mensajes privados.
• Secuestro de Sesión (Session Hijacking): Al viajar las cookies de autenticación expuestas, un tercero puede interceptarlas y replicarlas en su propio navegador para suplantar la identidad de la víctima en portales activos sin necesidad de conocer su contraseña.

5. Cómo Ayuda una VPN (Solución Tecnológica)

La implementación de una Red Privada Virtual (VPN) altera radicalmente el escenario de riesgo a través de dos mecanismos de defensa multicapa:
• El Túnel Seguro (Tunelización): La VPN encapsula todo el tráfico IP saliente del dispositivo antes de que sea transmitido al enrutador de la Wi-Fi pública. Para cualquier observador o atacante intermedio (Sniffer), el origen y el destino real de los paquetes quedan completamente abstractos; el ISP o atacante solo detectará un flujo de datos dirigido hacia el nodo IP del servidor VPN.
• Cifrado de Extremo a Extremo: Toda la información en tránsito se procesa mediante algoritmos de cifrado simétrico robusto. Aunque el atacante logre interceptar los paquetes en el aire, solo obtendrá ruido criptográfico de alta entropía e ilegible, garantizando de esta forma la privacidad y la integridad de los datos.

6. Mis 3 Reglas de Oro para Navegar en Wi-Fi Públicas

Para mitigar riesgos críticos en redes de acceso abierto, se deben aplicar de forma estricta las siguientes directrices operativas:
1. Uso Obligatorio de VPN con Kill Switch Activo: Nunca transmitir datos corporativos o personales sin levantar un túnel VPN. Se debe asegurar el uso de la función Kill Switch para interrumpir el tráfico de internet de inmediato si el túnel criptográfico sufre una caída accidental.
2. Verificación Estricta de HTTPS y Desactivación de Compartición: Validar la presencia del candado de seguridad TLS en la barra de direcciones. Paralelamente, configurar el perfil de red del sistema operativo en modo "Público" para desactivar la compartición de archivos, impresoras y el descubrimiento de redes locales.
3. Higienización de Conexiones (Olvidadas y Actualizadas): Eliminar permanentemente del dispositivo el historial de redes Wi-Fi abiertas utilizadas con anterioridad (Olvidadas) para prevenir que el equipo se conecte de forma automática a puntos de acceso no controlados o a redes Evil Twins. Mantener los sistemas operativos y navegadores al día con sus parches de seguridad críticos.
