# Makefile per la compilazione out-of-tree del driver hid-magicmouse
# Questo permette di testare la patch senza ricompilare tutto il kernel

obj-m += hid-magicmouse.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
