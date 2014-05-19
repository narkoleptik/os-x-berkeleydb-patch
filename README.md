os-x-berkeleydb-patch
=====================

Xcode 4.6 throws an error when compiling berkeley db 5.3.21, so here's a ptach

When compiling, I got this error
--------------------------------
<pre><code>
{bmcduffi@breezy.local}{Tue Mar 12 12:49:44}{~/src/db-5.3.21/build_unix}
√ $ make
./libtool --mode=compile cc -c -I. -I../src  -O3  ../src/mutex/mut_tas.c
libtool: compile:  cc -c -I. -I../src -O3 ../src/mutex/mut_tas.c  -fno-common -DPIC -o .libs/mut_tas.o
In file included from ../src/mutex/mut_tas.c:11:
In file included from ./db_int.h:1113:
In file included from ../src/dbinc/mutex.h:15:
In file included from ../src/dbinc/mutex_int.h:12:
../src/dbinc/atomic.h:179:19: error: definition of builtin function '__atomic_compare_exchange'
static inline int __atomic_compare_exchange(
                   ^
1 error generated.
make: *** [mut_tas.lo] Error 1
</code></pre>

### I'm sure there are more elegant ways to patch this, but here are my steps:
1. <pre><code>curl -O http://download.oracle.com/berkeley-db/db-5.3.21.tar.gz</code></pre>
1. <pre><code>tar -xvf db-5.3.21.tar.gz</code></pre>
1. <pre><code>cd db-5.3.21/</code></pre>

### We need to patch the code to make it compile on Xcode 4.6
1. <pre><code>curl -OL https://raw.github.com/narkoleptik/os-x-berkeleydb-patch/master/atomic.patch</code></pre>
1. <pre><code>patch src/dbinc/atomic.h < atomic.patch</code></pre>

### Back to building BerkeleyDB
1. <pre><code>cd build_unix</code></pre>
1. <pre><code>../dist/configure --prefix=/Users/bmcduffi</code></pre>
1. <pre><code>make</code></pre>
1. <pre><code>make install</code></pre>
