SRCS=$(wildcard fs_*.c)
OBJC=$(SRCS:%.c=%.o)
DEPS=$(OBJC:%.o=%.d)
OUT=$(OBJC:%.o=%)
ARGS=-Wall -Wextra -g -MMD
FS_NAME=fs
COMMON=../common
COMMON_LIBC=$(COMMON)/common_libc.o

.SECONDARY:

all: $(OUT)

fs.img: fs_create fs_add txt_files/*
	./fs_create doge_fs 1024 $(FS_NAME).img 307200
	./add_files.sh

fs_%: fs_%.o tools.o $(COMMON_LIBC)
	gcc $(ARGS) $^ -o $@

%.o: %.c
	gcc $(ARGS) -c $<

$(COMMON_LIBC):
	$(MAKE) common_libc.o -C $(COMMON)

clean:
	rm -rf $(OUT) *.o *.d *.img

test_fs_create_big: fs_create
	./fs_create doge_fs 512 $(FS_NAME).img 2147483136

test_fs_create_medium: fs_create
	./fs_create doge_fs 1024 $(FS_NAME).img 307200

test_fs_create_small: fs_create
	./fs_create doge_fs 512 $(FS_NAME).img 6656

test_fs_add: fs_add
	./fs_add txt_files/AARON.txt $(FS_NAME).img

test_fs_list: fs_list
	./fs_list $(FS_NAME).img

test_fs_del: fs_del
	./fs_del hello.txt $(FS_NAME).img

test_fs_info: fs_info
	./fs_info $(FS_NAME).img

test_fs_add_files: fs_add fs_del
	./scenario.sh

rebuild: clean all

help:
	@echo "Available targets :"
	@echo "\tall (default) : compile $(OUT)"
	@echo "\tfs.img : create fs.img and add txt_files files"
	@echo "\thelp : this help"
	@echo "\t$(COMMON_LIBC) : compile the common.o file with the stdio lib"
	@echo "\ttest_fs_* : multiple rules to test all tools (see in makefile for more details)"
	@echo "\tclean : remove $(OUT), *.o, *.d and *.img files"
	@echo "\trebuild : call clean and all"

.PHONY: clean help rebuild

-include $(DEPS)
