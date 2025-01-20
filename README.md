# CRUD_STOCK
CRUD utilizando Python/SQL/Pandas


import sqlite3
import pandas as pd

df_producto= pd.DataFrame()

def conexion():
    conn= sqlite3.connect("stock.db")
    cursor= conn.cursor()
    cursor.execute("""CREATE TABLE IF NOT EXISTS productos (id INTEGER PRIMARY KEY AUTOINCREMENT, nombre TEXT NOT NULL, cantidad INTEGER NOT NULL, precio REAL NOT NULL)""")
    conn.commit()
    conn.close()
    
conexion()
print("La conexion se creo correctamente")


def agregar_productos(nombre,cantidad,precio):
    conn= sqlite3.connect("stock.db")
    cursor= conn.cursor()
    cursor.execute("INSERT INTO productos(nombre,cantidad,precio)VALUES(?,?,?)",(nombre,cantidad,precio))
    conn.commit()
    conn.close()
    print(f"Producto '{nombre}' agregado correctamente")
    
def actualizar_stock(id_producto,nueva_cantidad):
    conn= sqlite3.connect("stock.db")
    cursor= conn.cursor()
    cursor.execute("UPDATE productos SET cantidad = ? WHERE id = ?", (nueva_cantidad,id_producto))
    conn.commit()
    conn.close()
    print(f"El Stock del producto {id} a sido actualizado")
    

def eliminar_producto(id_producto):
    conn= sqlite3.connect("stock.db")
    cursor= conn.cursor()
    cursor.execute("DELETE FROM productos WHERE id = ?",(id_producto,))
    conn.commit()
    conn.close()
    print(f"El producto {id_producto} a sido eliminado")
    
    
def consulta_stock():
    conn= sqlite3.connect("stock.db")
    cursor= conn.cursor()
    cursor.execute("SELECT * FROM productos")
    productos= cursor.fetchall()
    conn.close()
    return productos

def exportar_csv():
    import pandas as pd
    conn=sqlite3.connect("stock.db")
    df= pd.read_sql_query("SELECT * FROM productos", conn)
    df.to_csv("stock.csv", index=False)
    conn.close()
    print(" Datos exportados a CSV para usar en Power bi")

#def analizar_stock():
 #   conn=sqlite3.connect("stock.db")
 #   cursor= conn.cursor()
#
 #   # Obtener el total de productos en stock
 #   cursor.execute("SELECT SUM(cantidad) FROM productos")
#    total_stock = cursor.fetchone()[0] or 1 # Evitar división por cero
#
#    # Obtener productos con mayor stock y su porcentaje
#   cursor.execute("SELECT nombre, cantidad FROM productos ORDER BY cantidad DESC")
#    productos = cursor.fetchall()
 #   return total_stock, productos 
#
#    print("\n--- Análisis de Stock ---")
 #   for nombre, cantidad in productos:
#        porcentaje = (cantidad / total_stock) * 100
 #       print(f"{nombre}: {cantidad} unidades ({porcentaje:.2f}%)")
        
    
        

def menu():
    while True:
        print("\n -----GESTION DE STOCK-----")
        print("1. Agregar Producto: ")
        print("2. Actualizar Producto: ")
        print("3. Eliminar Producto: ")
        print("4. Consultar Producto: ")
        print("5. Exportar Productos a CSV: ")
        #print("6. Analizar Stock")
        print("0. Salir: ")
        
        opcion= int(input("Seleccione una Opción: "))
                       
        if opcion == 1:
            nombre= input("Nombre del Producto: ")
            cantidad= int(input("Ingrese la cantidad del Producto: "))
            precio= float(input("Ingrese el precio del Producto: "))
            agregar_productos(nombre,cantidad,precio)
        elif opcion==2:
            id_producto= int(input("ID del Producto: "))
            nueva_cantidad= int(input("Ingrese la nueva cantidad: "))
            actualizar_stock(id_producto,nueva_cantidad)
            
        elif opcion ==3:
            id_producto= int(input("ID del Producto: "))
            eliminar_producto(id_producto)
            
        elif opcion == 4:
            producto= consulta_stock()
            
            df_producto = pd.DataFrame(producto, columns=['ID', 'Nombre', 'Cantidad', 'Precio'])
            print(df_producto)
            #for d in producto:
             #   print(f"ID: {d[0]}, Nombre: {d[1]}, Cantidad:{d[2]},Precio: {d[3]}")
            print(input("Ingrese una tecla para continuar"))
            
                
        elif opcion==5:
            exportar_csv()
            
        elif opcion==6:
            analizar_stock()

            
        elif opcion == 0:
            print("Saliendo.....")
            break
        else:
            print("Opcion no valida")
            
            
if __name__ == '__main__':
    menu()
    
