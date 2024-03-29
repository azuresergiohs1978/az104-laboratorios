---
lab:
  title: '03d: Administración de recursos de Azure mediante la CLI de Azure'
  module: Module 03 - Azure Administration
ms.openlocfilehash: 7ca60ef19cd8712a95a957c710d10810d05be4ad
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625580"
---
# <a name="lab-03d---manage-azure-resources-by-using-azure-cli"></a>Laboratorio 03d: Administración de recursos de Azure mediante la CLI de Azure
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

Ahora que ha explorado las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos mediante Azure Portal, las plantillas de Azure Resource Manager y Azure PowerShell, debe llevar a cabo la tarea equivalente mediante la CLI de Azure. Para evitar la instalación de la CLI de Azure, aprovechará el entorno de Bash disponible en Azure Cloud Shell.

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Iniciar una sesión de Bash en Azure Cloud Shell
+ Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure
+ Tarea 3: Configurar el disco administrado mediante la CLI de Azure

## <a name="estimated-timing-20-minutes"></a>Tiempo estimado: 20 minutos

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-start-a-bash-session-in-azure-cloud-shell"></a>Tarea 1: Iniciar una sesión de Bash en Azure Cloud Shell

En esta tarea, abrirá una sesión de Bash en Cloud Shell. 

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **Bash**. 

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**. 

1. Si se le solicite, haga clic en **Crear almacenamiento** y espere hasta que aparezca Azure Cloud Shell. 

1. Asegúrese de que **Bash** aparezca en el menú desplegable superior izquierdo del panel de Cloud Shell.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-cli"></a>Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure

En esta tarea, creará un grupo de recursos y un disco administrado de Azure mediante una sesión de la CLI de Azure dentro de Cloud Shell.

1. Para crear un grupo de recursos en la misma región de Azure que el grupo de recursos **az104-03c-rg1** que creó en el laboratorio anterior, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
1. Para recuperar las propiedades del grupo de recursos recién creado, ejecute lo siguiente:

   ```sh
   az group show --name $RGNAME
   ```
1. Para crear un nuevo disco administrado con las mismas características que las que creó en los laboratorios anteriores de este módulo, desde la sesión de Bash dentro de Cloud Shell, ejecute lo siguiente:

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
    >**Nota**: Al usar la sintaxis de varias líneas, asegúrese de que cada línea termine con barra diagonal inversa (`\`) sin espacios finales, y de que no haya espacios iniciales al principio de cada línea.

1. Para recuperar las propiedades del disco recién creado, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```

#### <a name="task-3-configure-the-managed-disk-by-using-azure-cli"></a>Tarea 3: Configurar el disco administrado mediante la CLI de Azure

En esta tarea, administrará la configuración del disco administrado de Azure mediante una sesión de la CLI de Azure en Cloud Shell. 

1. Para aumentar el tamaño del disco administrado de Azure a **64 GB**, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGb
   ```

1. Para cambiar la SKU de rendimiento del disco a **Premium_LRS**, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

#### <a name="clean-up-resources"></a>Limpieza de recursos

   >**Nota**: No olvide quitar los recursos de Azure recién creados que ya no use. La eliminación de los recursos sin usar garantiza que no verá cargos inesperados.

1. En Azure Portal, abra la sesión de shell de **Bash** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro --nowait). Aunque podrá ejecutar otro comando de la CLI de Azure inmediatamente después en la misma sesión de Bash, los grupos de recursos tardarán unos minutos en quitarse.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Iniciado una sesión de Bash en Azure Cloud Shell
- Creado un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure
- Configurado el disco administrado mediante la CLI de Azure
