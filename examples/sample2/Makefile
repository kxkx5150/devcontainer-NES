CL65	= cl65
LD65	= ld65

#-------------------------------------------------------------------------------
CSOURCES =

ASMSOURCES =	sample2.asm

OBJECTS	=	$(CSOURCES:.c=.o) $(ASMSOURCES:.asm=.o)

LIBRARIES =
#-------------------------------------------------------------------------------
all :	$(OBJECTS) $(LIBRARIES)
	$(LD65) -o sample2.nes --config sample2.cfg --obj $(OBJECTS)

.SUFFIXES : .asm .o

.c.o :
	$(CL65) -t none -o $*.o -c -O $*.c

.asm.o :
	$(CL65) -t none -o $*.o -c $*.asm

clean :
	rm *.nes
	rm *.o
