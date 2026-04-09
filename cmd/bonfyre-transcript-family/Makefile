CC ?= cc
CFLAGS ?= -O2 -Wall -Wextra -std=c11
LOCAL_CFLAGS = -I../../vendor/bonfyre-core/lib/libbonfyre/include
LDFLAGS  = -L../../vendor/bonfyre-core/lib/libbonfyre -lbonfyre

all: bonfyre-transcript-family

bonfyre-transcript-family: src/main.c
	$(CC) $(CFLAGS) $(LOCAL_CFLAGS) -o $@ src/main.c $(LDFLAGS)

clean:
	rm -f bonfyre-transcript-family
