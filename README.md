# Detener-Volley-antes-de-terminal-Android

Para cancelar una solicitud, llame ```cancel()``` a su Requestobjeto. Una vez cancelado, Volley garantiza que nunca se llamará a su controlador de respuesta. Lo que esto significa en la práctica es que puede cancelar todas sus solicitudes pendientes en el ```onStop()``` método de su actividad y no tiene que ensuciar a sus manejadores de respuestas con verificaciones ```getActivity() == null``` , ya sea que ya ```onSaveInstanceState()``` se haya llamado, u otro texto repetitivo defensivo.

Para aprovechar este comportamiento, normalmente debe realizar un seguimiento de todas las solicitudes en vuelo para poder cancelarlas en el momento apropiado. Hay una manera más fácil: puede asociar un objeto de etiqueta con cada solicitud. A continuación, puede usar esta etiqueta para proporcionar un alcance de solicitudes para cancelar. Por ejemplo, puede etiquetar todas sus solicitudes con las ```Activity``` que se están realizando en nombre de, y llamar ```requestQueue.cancelAll(this)``` desde ```onStop()```. Del mismo modo, puede etiquetar todas las solicitudes de imágenes en miniatura en una ```ViewPager``` pestaña con sus respectivas pestañas y cancelar al deslizar para asegurarse de que la nueva pestaña no esté retenida por las solicitudes de otra.

Aquí hay un ejemplo que usa un valor de cadena para la etiqueta:

1 - Define tu etiqueta y agrégala a tus solicitudes.

```
public static final String TAG = "MyTag";
StringRequest stringRequest; // Assume this exists.
RequestQueue mRequestQueue;  // Assume this exists.

// Set the tag on the request.
stringRequest.setTag(TAG);

// Add the request to the RequestQueue.
mRequestQueue.add(stringRequest);
```

2 - En el ```onStop()``` método de su actividad , cancele todas las solicitudes que tengan esta etiqueta.

```
@Override
protected void onStop () {
    super.onStop();
    if (mRequestQueue != null) {
        mRequestQueue.cancelAll(TAG);
    }
}
```
