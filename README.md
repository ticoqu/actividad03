# Actividad 3: Crear una máquina virtual básica con Cloud-Init

## Descripción
Aquí crearás una máquina virtual simple usando Terraform y un archivo cloud-init para configuraciones iniciales.

## Requisitos
- Terraform instalado.
- Plantilla `debian-12-qcow2-template` en Proxmox.

### 🎯 Objetivo
Desplegar una máquina virtual básica en Proxmox usando Terraform y Cloud-Init.

### Entrega
- Captura de pantalla del estado final de terraform apply.
- Comprobar acceso por SSH a la VM.

## Pasos a seguir

1. **Crear el archivo `main.tf`**
```hcl
   resource "proxmox_vm_qemu" "vm_basica" {
  name        = "vm-basica"
  target_node = "red-33-proxmox-1"
  desc        = "VM básica con cloud-init"
  clone       = "debian-12-qcow2-template"
  cores       = 2
  sockets     = 1
  memory      = 2048
  scsihw      = "virtio-scsi-pci"
  agent       = 1
  qemu_os     = "l26"
  ciuser      = "automatizacion"
  cipassword  = "12345"
  searchdomain = "agetic.gob.bo"
  nameserver  = "8.8.8.8"
  ipconfig0   = "ip=192.168.33.20/24,gw=192.168.33.1"

  disks {
    virtio {
      virtio0 {
        disk {
          storage = "vol1"
          size    = "8G"
        }
      }
    }
    ide {
      ide2 {
        cloudinit {
          storage = "vol1"
        }
      }
    }
  }

  network {
    model  = "virtio"
    bridge = "vmbr0"
    mtu    = 1450
  }
}
```

2. **Verificar tareas**
   ```bash
   terraform plan
   ```
   
3. **Ejecutar el despliegue**
   ```bash
   terraform apply
   ```
