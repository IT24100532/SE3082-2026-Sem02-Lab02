# Makefile
CC = gcc
CFLAGS = -Wall

myapp: main.o utils.o
	$(CC) $(CFLAGS) -o myapp main.o utils.o

main.o: main.c
	$(CC) $(CFLAGS) -c main.c

utils.o: utils.c
	$(CC) $(CFLAGS) -c utils.c

clean:
	rm -f *.o myapp
