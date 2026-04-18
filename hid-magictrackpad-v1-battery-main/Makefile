# Makefile per la compilazione out-of-tree del driver hid-magicmouse
# Questo permette di testare la patch senza ricompilare tutto il kernel

obj-m += hid-magicmouse.o
PWD := $(shell pwd)
KDIR := /lib/modules/$(shell uname -r)/build

all:
	$(MAKE) -C $(KDIR) M=$(PWD) modules

clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean
