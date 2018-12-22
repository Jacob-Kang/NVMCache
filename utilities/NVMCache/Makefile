NVM_CACHE_NAME=NVMCache
uname_S := $(shell sh -c 'uname -s 2>/dev/null || echo not')

STD=-std=c++11  
WARN=-Wall -W
OPTIMIZATION?=-O0
OPT=$(OPTIMIZATION) -DROCKSDB_PLATFORM_POSIX
ifeq ($(uname_S),Darwin)
	OPT+=-DOS_MACOSX
endif

DEBUG=-g -ggdb

FINAL_CFLAGS=$(STD) $(WARN) $(OPT) $(DEBUG) $(CFLAGS) $(REDIS_CFLAGS)
FINAL_CFLAGS+= -I../../include -I../../

NVMCACHE_CC=$(CC) $(FINAL_CFLAGS)

NVM_CACHE_OBJ=persistent_cache.o NVMCache_file.o NVMCache.o NVMCache_metadata.o 

all: $(NVM_CACHE_NAME)

$(NVM_CACHE_NAME): $(NVM_CACHE_OBJ)
	$(NVMCACHE_CC) -o $@ $^ -lhiredis -llua -lrocksdb_debug $(ROCKSDB_LIBS) $(FINAL_LIBS)

%.o: %.cc
	$(CXX) $(FINAL_CFLAGS) -c -o $@ $< 

clean:
	rm -rf *.o