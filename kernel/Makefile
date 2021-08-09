KERNEL := kernel.elf

CC = clang -target aarch64-none-elf
LD = ld.lld

CFLAGS = -Wall -Wextra -O2 -pipe
LDFLAGS =

INTERNALLDFLAGS :=         \
	-Tlinker.ld            \
	-nostdlib              \
	-zmax-page-size=0x1000 \
	-static                \
	-pie                   \
	--no-dynamic-linker    \
	-ztext

INTERNALCFLAGS  :=       \
	-I.                  \
	-std=gnu11           \
	-ffreestanding       \
	-fno-stack-protector \
	-fno-pic -fpie       
#	-mno-80387           \
#	-mno-mmx             \
#	-mno-3dnow           \
#	-mno-sse             \
#	-mno-sse2            \
#	-mno-red-zone

CFILES := $(shell find ./ -type f -name '*.c')
SFNFILES := $(shell find ./ -type f -name '*.sfn')
OBJ    := $(CFILES:.c=.o)
SFNOBJ := $(SFNFILES:.sfn=.sfn.o)

.PHONY: all clean

all: $(KERNEL)

$(KERNEL): $(OBJ) $(SFNOBJ)
	$(LD) $(OBJ) $(SFNOBJ) $(LDFLAGS) $(INTERNALLDFLAGS) -o $@

%.o: %.c 
	$(CC) $(CFLAGS) $(INTERNALCFLAGS) -c $< -o $@

%.sfn.o: %.sfn
	llvm-objcopy -I binary -O elf64-aarch64 $< $@

clean:
	rm -rf $(KERNEL) $(OBJ)
