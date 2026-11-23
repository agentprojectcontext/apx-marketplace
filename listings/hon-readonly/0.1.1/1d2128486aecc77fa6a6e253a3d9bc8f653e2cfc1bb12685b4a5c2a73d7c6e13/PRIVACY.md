# hOn read-only extension privacy

This is an unofficial community connector, not hOn's privacy policy. Review the hOn app's current account privacy notice and service terms before connecting. The website currently links its privacy notice at http://www.candy-group.com/privacy/V1/privacy.html; that destination's availability and transport are controlled by the service, not APX. The listing's privacy link is APX's general privacy reference; this file describes the wrapper specifically. The extension's Terms link is the APX wrapper's MIT license, not a substitute for hOn's service terms.

APX injects this extension's hOn email and password from its own secret fields at runtime. They are sent through pyhOn to upstream authentication endpoints. Session tokens remain in the Python process; this wrapper does not save them to disk. pyhOn's diagnostic logging is disabled because authentication responses can contain private data.

Appliance listing and attribute requests go to the hOn cloud. The tool output deliberately omits names, free-text models, MACs, serials, unknown parameter names, raw responses and nested data. Only known appliance type/connection codes, a temporary position number and bounded numeric telemetry are returned, with query time and fixed error categories. Numeric values can still describe household activity. Selected tool results may be retained in APX chat history and sent to the model provider configured by the owner. There is no additional analytics recipient.

uv downloads the locked dependencies and, if needed, Python into its own cache. Download hosts receive ordinary network metadata. This extension does not control the upstream cloud's retention, account handling or security. Turning it off stops its tool process; removing it does not delete the hOn account or APX's existing chat history.

## Español

Este aviso corresponde al conector comunitario no oficial, no a la política de hOn. Revisá el aviso de privacidad y las condiciones de la app hOn antes de conectar. La web de hOn enlaza actualmente el aviso indicado arriba; su disponibilidad y transporte dependen del servicio. El enlace de condiciones de la ficha es la licencia MIT del wrapper, no reemplaza las condiciones hOn.

APX inyecta el email y la contraseña propios de esta extensión al ejecutar; pyhOn los envía a la autenticación del servicio. Los tokens quedan en memoria y el wrapper no los guarda en disco. Los logs de pyhOn están desactivados. Las consultas van a la nube hOn. El resultado omite nombres, modelos libres, MAC, seriales, parámetros desconocidos, respuestas crudas y objetos anidados. Sólo devuelve códigos conocidos, posiciones temporales y telemetría numérica acotada, con hora de consulta y categorías fijas de error. Estos datos pueden describir actividad del hogar y quedar en el chat o llegar al proveedor de IA elegido. No agrega analítica.

uv descarga dependencias fijadas y, si hace falta, Python en su caché; los servidores reciben metadatos normales de red. La extensión no controla la retención ni la seguridad del servicio. Apagarla detiene el proceso; quitarla no borra la cuenta hOn ni el historial de APX.
