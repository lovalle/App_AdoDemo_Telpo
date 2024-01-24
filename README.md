# Lanzador de ejemplo para integración de la libreria scanovate_colombia

El lanzador es un ejemplo de implementación de las librerias necesarias para iniciar el proceso de validación.

## Instalación

Primero, añadir las librerías "scanovate_colombia_2_2_2", "libScanovateImagingHybridLiveness_2_2_6", "ScanovateManualCapture_1_0_7"
en las dependencias del proyecto. 

`dependencies{
  implementation(name: 'scanovate_colombia_2_2_2', ext: 'aar')
  implementation(name: 'libScanovateImagingHybridLiveness_2_2_6', ext: 'aar')
  implementation(name: 'ScanovateManualCapture_1_0_7', ext: 'aar')
  }`
  
Asi mismo se podrán importar las siguientes librerías.

`import mabel_tech.com.scanovate_demo.ScanovateHandler;
 import mabel_tech.com.scanovate_demo.ScanovateSdk;
 import mabel_tech.com.scanovate_demo.model.CloseResponse;
 import mabel_tech.com.scanovate_demo.network.ApiHelper;
 import mabel_tech.com.scanovate_demo.network.RetrofitClient;`

La librería responde el resultado de la transacción en un objeto llamado CloseResponse  

### Version minima del SDK Android

Cambiar la versión minima del SDK Android a 21 (o mas alta) en el archivo `android/app/build.gradle`

### Ejemplo

Este es un pequeño ejemplo de como invocar el metodo que lanzara la librería. 

    package com.mabel_tech.scanovate_colombia_sdk_demo;
    
    import android.app.ProgressDialog;
    import android.content.Context;
    import android.os.Bundle;
    import android.support.annotation.Nullable;
    import android.support.v7.app.AppCompatActivity;
    import android.view.View;
    import android.widget.Button;
    import android.widget.CheckBox;
    import android.widget.EditText;
    import android.widget.TextView;
    import mabel_tech.com.scanovate_demo.ScanovateHandler;
    import mabel_tech.com.scanovate_demo.ScanovateSdk;
    import mabel_tech.com.scanovate_demo.model.CloseResponse;
    import mabel_tech.com.scanovate_demo.network.ApiHelper;
    import mabel_tech.com.scanovate_demo.network.RetrofitClient;
    import okhttp3.ResponseBody;
    
    
    public class MainActivity extends AppCompatActivity {
    
        private ProgressDialog progress;
    
        Button btn_enrolar;
        Button btn_verificar;
        TextView tv;
        CheckBox checkBox;
        Context contect;
        EditText numberId;
        String numberIdentification;
        Boolean verification;
    
    
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            tv = findViewById(R.id.textView);
            progress = new ProgressDialog(this);
            progress.setTitle("Procesando estado");
            progress.setMessage("Por favor espere un momento...");
            progress.setIndeterminate(true);
            progress.setCanceledOnTouchOutside(false);
            contect = this;
            btn_enrolar = findViewById(R.id.btn_enroll);
            btn_verificar = findViewById(R.id.btn_verification);
            numberId = findViewById(R.id.numberId);
            verification = false;
            initView();
    
        }
    
        public void initView() {
    
            btn_enrolar.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    capture();
                }
            });
    
            btn_verificar.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if (!btn_verificar.getText().equals("Enviar")) {
                        numberId.setVisibility(View.VISIBLE);
                        btn_enrolar.setVisibility(View.INVISIBLE);
                        btn_verificar.setText("Enviar");
                    } else {
                        if (numberId.getText().length() != 0) {
                            numberIdentification = numberId.getText().toString();
                            verification = true;
                            numberId.setVisibility(View.INVISIBLE);
                            btn_verificar.setVisibility(View.INVISIBLE);
                            capture();
    
                        }
    
                    }
                }
            });
        }
    
        public void capture() {
            ScanovateSdk.start(this,
                    "1",
                    1,
                    "progreserqa",
                    "db92efc69991",
                    "https://adocolumbia.ado-tech.com/progreserqa/api/",
                    numberIdentification,
                    verification,
                    "",
                    "",
                    new ScanovateHandler() {
                        @Override
                        public void onSuccess(CloseResponse response, int code, String uuidDevice) {
                            progress.show();
                            String calificacion = response.getExtras().getStateName();
                        }
    
                        @Override
                        public void onFailure(CloseResponse closeResponse) {
                            String calificacion = closeResponse.getExtras().getStateName() +" "+ closeResponse.getExtras().getAdditionalProp1() ;
                        }
    
    
                    });
        }
    }
# AdoDemo_Telpo_Android
