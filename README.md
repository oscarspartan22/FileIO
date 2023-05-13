# FileIO Editor de archivos

# Proyecto: Aplicación que permita cargar un archivo de tipo .json, Editarlo y eliminarlo
## Autor: Oscar Omar Quijano Cruz

Se elabora el JFrame con los elementos.

Se incorporan los siguientes elementos con formato: un área de texto (TxtArea) que mostrará el contenido del archivo seleccionado y permitirá editar el archivo en ese mismo lugar. Un botón (Archivo) para buscar y seleccionar el archivo, un botón (Guardar) para guardar los cambios realizados en el archivo y un botón (Eliminar) para eliminar el archivo. Además, se ha agregado un ComboBox para seleccionar el tipo de archivo a buscar.

![image](https://github.com/oscarspartan22/FileIO/assets/131202385/5a34cf8f-468b-4b8d-8198-d16b9b5bafb7)

# Agregar funcionalidad al botón archivo al dar clic

Se implementa un JFileChooser para que al hacer clic en el botón correspondiente se abra un panel de selección de archivo. Se establece un "filtro" con el FileNameExtensionFilter para especificar qué tipo de archivo se debe abrir, el cual se obtiene del ComboBox donde se puede elegir entre txt o json. A continuación, se muestra el cuadro correspondiente. Para manejar los resultados de la selección de archivo, se utiliza una estructura de control if para no hacer nada si se selecciona "cancelar" y proceder si se selecciona "abrir". Entonces, se muestra la ruta del archivo seleccionado en un textField y se utiliza un ciclo while para insertar carácter por carácter en una cadena de caracteres (string) para luego mostrar el contenido completo del archivo en un textArea. Se coloca todo el proceso dentro de un bloque try-catch para evitar errores de ejecución.

```

private void btnArchivoMouseClicked(java.awt.event.MouseEvent evt) {                                        
    System.out.println("Inico de la carga de archivo");
    JFileChooser fc = new JFileChooser();
    FileNameExtensionFilter filter = new FileNameExtensionFilter("Archivos de texto o json",cmboxTipoArchivo.getSelectedItem().toString());

    fc. setFileFilter(filter);
    int seleccion = fc. showOpenDialog(this);
    if(seleccion == JFileChooser.CANCEL_OPTION){

    }else if(seleccion == JFileChooser.APPROVE_OPTION){
        File archivoSeleccionado = fc.getSelectedFile();
        this.txtArchivo.setText( archivoSeleccionado.getAbsolutePath());

        try( FileReader fr = new FileReader(archivoSeleccionado)){
            String cadena = "";
            int valor = fr.read();
            while (valor != -1){
                cadena += (char) valor;
                valor = fr.read();
            }
            this.txtAreaEntrada.setText(cadena);
        } catch(IOException e){
    e.printStackTrace();
        }
    }
} 

```

# Agregar funcionalidad al botón guardar al dar clic

En primer lugar, se incluye una estructura de control if para verificar si el textField está vacío. Si esto es así, significa que no se ha importado ningún archivo, por lo que cuando se presiona el botón "guardar" se muestra un cuadro de diálogo. En caso contrario, si se ha mostrado un archivo, los cambios realizados en el TxtArea se guardarán en el mismo archivo. Para hacer esto, se crea un objeto "file" y se utiliza PrintWriter para escribir dentro de él, utilizando el método print() con el contenido del TxtArea como argumento. Finalmente, se cierra el archivo utilizando el método close() para evitar cambios posteriores. Todo esto se coloca dentro de un bloque try-catch para manejar cualquier excepción de tipo FileNotFoundException y mostrarlo en la consola con una descripción.
```
private void btnGuardarMouseClicked(java.awt.event.MouseEvent evt) {                                        
    if(txtArchivo.getText().isEmpty()){
        JOptionPane.showMessageDialog(null, "Seleccione un archivo del tipo requerido");
    }else {
        System.out.println("Inico guardar archivo");
        File archivo = new File(this.txtArchivo.getText());
        PrintWriter escribir;
        try {
            escribir = new PrintWriter(archivo);
            escribir.print(txtAreaEntrada.getText());
            escribir.close();
        } catch (FileNotFoundException ex) {
        // Logger.getLogger(FrmPrincipal.class.getName()).log(Level.SEVERE, null, ex);
        java.util.logging.Logger.getLogger(EditorFiles.class.getName()).log(java.util.logging.Level.SEVERE
        , null, ex);

        }
    }
}
```

# Agregar funcionalidad al botón eliminar al dar clic

Aqui con el método delete se borra el archivo importado. Se usa el if para que si no se pudo borrar (ya sea problamento porque no se importó nada) muestre un mensaje.

```
private void btnEliminarMouseClicked(java.awt.event.MouseEvent evt) {                                         
    System.out.println("Inico eliminar archivo");
    File archivo = new File(this.txtArchivo.getText());
    if (archivo.delete()) {
        System.out.println("Archivo borrado: " + archivo.getName());
        txtArchivo.setText("");
    } else {
        System.out.println("Error al borrar el archivo");
    }
}                                        

```
# Funcionalidad del programa. 

1. Al dar clic en el botón Archivo, se busca un archivo json y se da a open:
![image](https://github.com/oscarspartan22/FileIO/assets/131202385/92b96309-e6e7-4aad-81ee-13117b0a5ac8)
![image](https://github.com/oscarspartan22/FileIO/assets/131202385/b5a9b13b-cb37-4567-841f-307d9f4eb6b9)

2. Se modifican texto en el txtArea y se guarda.
![image](https://github.com/oscarspartan22/FileIO/assets/131202385/7456d30e-5396-49dd-a850-cebe24f0c6fe)
![image](https://github.com/oscarspartan22/FileIO/assets/131202385/ad7a3054-52aa-49b1-9225-ef56cbe6cba7)

3. Se elimina el archivo.

![image](https://github.com/oscarspartan22/FileIO/assets/131202385/83e59499-2ff4-4b36-9dfb-91fe245b2704)
![image](https://github.com/oscarspartan22/FileIO/assets/131202385/3968134d-c11f-46a2-ac9a-9b9e08475c8e)
