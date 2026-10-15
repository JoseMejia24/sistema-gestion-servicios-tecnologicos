# main.py

class Servicio:
    """Clase base que representa un servicio tecnológico general."""
    def __init__(self, codigo: str, cliente: str, descripcion: str, precio_base: float):
        self.codigo = codigo
        self.cliente = cliente
        self.descripcion = descripcion
        self.precio_base = precio_base

    def calcular_costo(self) -> float:
        """Método abstracto/base para el polimorfismo."""
        return self.precio_base

    def __str__(self) -> str:
        return (f"Código: {self.codigo} | Cliente: {self.cliente} | "
                f"Descripción: {self.descripcion} | Precio Base: ${self.precio_base:.2f}")


class Mantenimiento(Servicio):
    """Clase derivada para servicios de Mantenimiento."""
    def __init__(self, codigo: str, cliente: str, descripcion: str, precio_base: float, 
                 cantidad_equipos: int, costo_por_equipo: float):
        # Reutilización de atributos mediante herencia
        super().__init__(codigo, cliente, descripcion, precio_base)
        self.cantidad_equipos = cantidad_equipos
        self.costo_por_equipo = costo_por_equipo

    def calcular_costo(self) -> float:
        # Polimorfismo: Regla de cálculo específica
        return self.precio_base + (self.cantidad_equipos * self.costo_por_equipo)

    def __str__(self) -> str:
        return super().__str__() + f" | Equipos: {self.cantidad_equipos} | Costo/Eq: ${self.costo_por_equipo:.2f}"


class Instalacion(Servicio):
    """Clase derivada para servicios de Instalación."""
    def __init__(self, codigo: str, cliente: str, descripcion: str, precio_base: float, 
                 cantidad_dispositivos: int, costo_por_dispositivo: float):
        super().__init__(codigo, cliente, descripcion, precio_base)
        self.cantidad_dispositivos = cantidad_dispositivos
        self.costo_por_dispositivo = costo_por_dispositivo

    def calcular_costo(self) -> float:
        return self.precio_base + (self.cantidad_dispositivos * self.costo_por_dispositivo)

    def __str__(self) -> str:
        return super().__str__() + f" | Dispositivos: {self.cantidad_dispositivos} | Costo/Disp: ${self.costo_por_dispositivo:.2f}"


class SoporteTecnico(Servicio):
    """Clase derivada para servicios de Soporte Técnico."""
    def __init__(self, codigo: str, cliente: str, descripcion: str, precio_base: float, 
                 horas_atencion: float, tarifa_por_hora: float):
        super().__init__(codigo, cliente, descripcion, precio_base)
        self.horas_atencion = horas_atencion
        self.tarifa_por_hora = tarifa_por_hora

    def calcular_costo(self) -> float:
        return self.precio_base + (self.horas_atencion * self.tarifa_por_hora)

    def __str__(self) -> str:
        return super().__str__() + f" | Horas: {self.horas_atencion} | Tarifa/Hora: ${self.tarifa_por_hora:.2f}"


# --- FUNCIONES DE VALIDACIÓN ---
def solicitar_float_positivo(mensaje: str) -> float:
    while True:
        try:
            valor = float(input(mensaje))
            if valor < 0:
                print("⚠️ Error: El valor no puede ser negativo. Intente de nuevo.")
                continue
            return valor
        except ValueError:
            print("⚠️ Error: Ingrese un número válido.")

def solicitar_int_positivo(mensaje: str) -> int:
    while True:
        try:
            valor = int(input(mensaje))
            if valor < 0:
                print("⚠️ Error: La cantidad no puede ser negativa. Intente de nuevo.")
                continue
            return valor
        except ValueError:
            print("⚠️ Error: Ingrese un número entero válido.")


# --- PROGRAMA PRINCIPAL (MENÚ) ---
def menu():
    # Colección única común para demostrar Polimorfismo
    servicios_registrados = []
    
    while True:
        print("\n" + "="*50)
        print(" SISTEMA DE GESTIÓN DE SERVICIOS TECNOLÓGICOS")
        print("="*50)
        print("1. Registrar servicio de Mantenimiento")
        print("2. Registrar servicio de Instalación")
        print("3. Registrar servicio de Soporte Técnico")
        print("4. Mostrar servicios registrados")
        print("5. Calcular y mostrar costo de cada servicio")
        print("6. Mostrar el costo total de todos los servicios")
        print("7. Salir")
        
        opcion = input("Seleccione una opción (1-7): ").strip()
        
        if opcion in ['1', '2', '3']:
            print("\n--- Registro de Datos Generales ---")
            codigo = input("Código del servicio: ").strip()
            cliente = input("Nombre del cliente: ").strip()
            descripcion = input("Descripción: ").strip()
            precio_base = solicitar_float_positivo("Precio base ($): ")
            
            if opcion == '1':
                cant_equipos = solicitar_int_positivo("Cantidad de equipos: ")
                costo_eq = solicitar_float_positivo("Costo por equipo ($): ")
                nuevo_servicio = Mantenimiento(codigo, cliente, descripcion, precio_base, cant_equipos, costo_eq)
            elif opcion == '2':
                cant_disp = solicitar_int_positivo("Cantidad de dispositivos: ")
                costo_disp = solicitar_float_positivo("Costo por dispositivo ($): ")
                nuevo_servicio = Instalacion(codigo, cliente, descripcion, precio_base, cant_disp, costo_disp)
            elif opcion == '3':
                horas = solicitar_float_positivo("Horas de atención: ")
                tarifa = solicitar_float_positivo("Tarifa por hora ($): ")
                nuevo_servicio = SoporteTecnico(codigo, cliente, descripcion, precio_base, horas, tarifa)
                
            servicios_registrados.append(nuevo_servicio)
            print("✅ Servicio registrado exitosamente.")
            
        elif opcion == '4':
            print("\n--- Servicios Registrados ---")
            if not servicios_registrados:
                print("No hay servicios registrados.")
            else:
                for s in servicios_registrados:
                    print(s)
                    
        elif opcion == '5':
            print("\n--- Costos por Servicio (Polimorfismo en acción) ---")
            if not servicios_registrados:
                print("No hay servicios registrados.")
            else:
                # Recorremos la lista llamando al mismo método sin importar el tipo exacto de objeto
                for s in servicios_registrados:
                    print(f"Servicio {s.codigo} ({type(s).__name__}) -> Costo Total: ${s.calcular_costo():.2f}")
                    
        elif opcion == '6':
            print("\n--- Costo Total General ---")
            if not servicios_registrados:
                print("Costo total: $0.00")
            else:
                total_general = sum(s.calcular_costo() for s in servicios_registrados)
                print(f"La suma de todos los servicios registrados es: ${total_general:.2f}")
                
        elif opcion == '7':
            print("¡Gracias por utilizar el sistema!")
            break
        else:
            print("⚠️ Opción no válida. Ingrese un número de 1 a 7.")

if __name__ == "__main__":
    menu()