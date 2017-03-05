# Compilador
 
El archivo `calculator.c` es un programa sencillo, que suma
dos números y los imprime en pantalla; así y todo compilarlo 
requiere un montón de pasos intermedios. Estos pasos los podemos 
clasificar en 4:

1. Pre-procesador: `make preprocessing`
2. Compilacion I: `make assembler`
3. Compilacion II: `make object`
4. Linkeo: `make executable`

Ejecutando cada una de las instrucciones de `make` van a poder
construir cada uno de los pasos intermedios.

Ejercicios:

En un archivo de texto `respuestas.md`:

1. Escriban qué esperan de cada uno de los pasos

	1. Pre-procesador: `make preprocessing`
	
	gcc -E calculator.c -o calculator.pp.c
	el pre procesador analiza todo lo que esta con # (include y macros).
	
	2. Compilacion I: `make assembler`
	
	convierte el codigo optimizado en lenguaje emsamblador.
	
	3. Compilacion II: `make object`
	
	gcc -c calculator.c -o calculator.o

	nm calculator.o
	(nm comando para interpretar archivos binarios)
	
	Genera un archivo de tipo binario.
	Especifica descriptores como en el ejemplo (T: simbolos de texto global / U: simbolos indefinidos)
	 
	
	000000000000003c T add_numbers
    0000000000000000 T main
					 U printf

	
	4. Linkeo: `make executable`
    gcc -v calculator.c -o calculator.e
    
    genera el archivo ejecutable.



2. ¿Qué agregó el preprocesador?

agrega todas las definiciones que estan en los include y expande las macros.

3. Identificar en la rutina de assembler las funciones

gcc -S -masm=intel calculator.c -o calculator.asm (para ver la sintaxis del ensambler de Intel)

------------parte del assembler----------------------------------
.LFE0:
	.size	main, .-main
	.globl	add_numbers
	.type	add_numbers, @function
add_numbers:
.LFB1:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	mov	DWORD PTR [rbp-4], edi
	mov	DWORD PTR [rbp-8], esi
	mov	eax, DWORD PTR [rbp-8]
	mov	edx, DWORD PTR [rbp-4]
	add	eax, edx
	pop	rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
 
-------------------------------------------------------
4. Explicar qué quieren decir los símbolos que se crean en el objeto.

Los simbolos que se crean en el objetos son los descriptores.
Por ejemplo:

    A :  Global absolute symbol.
    a  :  Local absolute symbol.
    B : Global bss symbol.
    b : Local bss symbol.
    D : Global data symbol.
    d : Local data symbol.
    f : Source file name symbol.
    L : Global thread-local symbol (TLS).
    l : Static thread-local symbol (TLS).
    T : Global text symbol.
    t  : Local text symbol.
    U : Undefined symbol.


5. ¿En qué se diferencian los símbolos del objeto y del ejecutable?

nm calculator.e (nm del ejecutable)

en el ejecutable se agregan descriptores que se necesitan para ejecutar en programa.
Por ejemplo:

000000000000003c T add_numbers
0000000000000000 T main
                 U printf
wtpc@LabRedesPC17:~/wtpc/HOcompiled/compilador$ nm calculator.e
0000000000400569 T add_numbers
0000000000601040 B __bss_start
0000000000601040 b completed.6973
0000000000601030 D __data_start
0000000000601030 W data_start
0000000000400470 t deregister_tm_clones
00000000004004e0 t __do_global_dtors_aux
0000000000600e18 t __do_global_dtors_aux_fini_array_entry
0000000000601038 D __dso_handle
0000000000600e28 d _DYNAMIC
0000000000601040 D _edata



(siéntanse libres, si quieren, de usar la sintaxis markdown. no hace falta)
