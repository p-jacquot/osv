.PHONY: module
module: threads

CFLAGS = -std=gnu99 -fpie -rdynamic -lpthread

threads: threads.c
	$(CC) -pie -o $@ $(CFLAGS) $(LDFLAGS) threads.c

clean:
	rm -f threads
