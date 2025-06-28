ARCH     := x86_64
TARGET   := memory.efi
OBJS     := main.o

EFIINC   := /usr/include/efi
EFIARCH  := $(EFIINC)/$(ARCH)

EFILIBDIR := /usr/lib/$(ARCH)-linux-gnu/gnu-efi
EFI_CRT_OBJS := $(EFILIBDIR)/crt0-efi-$(ARCH).o
EFI_LDS      := $(EFILIBDIR)/elf_$(ARCH)_efi.lds

CFLAGS := -I$(EFIINC) -I$(EFIARCH) -I$(EFIINC)/protocol \
	 -fno-stack-protector -fpic -fshort-wchar -mno-red-zone \
	 -Wall -Wno-incompatible-pointer-types

LDFLAGS := -nostdlib -znocombreloc -T $(EFI_LDS) -shared \
	   -Bsymbolic -L$(EFILIBDIR) $(EFI_CRT_OBJS)

all: $(TARGET)

memory.so: $(OBJS)
	ld $(LDFLAGS) $(OBJS) -o $@ -lefi -lgnuefi

%.efi: %.so
	objcopy -j .text -j .sdata -j .data -j .dynamic \
		-j .dynsym  -j .rel -j .rela -j .reloc \
		--target=efi-app-$(ARCH) $^ $@

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(TARGET) memory.so $(OBJS) *~
