# Installing x0 From source

### Build-time Dependencies

- GCC 4.6 or 4.7
- cmake 2.8 or higher
- cppunit 1.12
- pod2man (part of Perl package) to generate man pages
- doxygen (optional)

### Runtime-time Dependencies

- libev 4.0
- libpcre
- libmysqlclient
- libgcrypt (optional)
- libgnutls 2.6 or 2.8 (optional)
- libbzip2 (optional)
- libgzip (optional)

### Installing x0

    git clone git://github.com/trapni/x0.git
    cd x0
    cmake -DCMAKE_BUILD_PREFIX=$HOME/local
    make && sudo make install

### First run!

There is an instant mode, where you can run x0d in a configuration-less mode
to just serve a single domain with its document root on one port with a
very basic setup predefined:

    $HOME/local/bin/x0d --instant=`pwd`,8080

However, a more elaborate startup might look like this:

    $HOME/local/bin/x0d --splash \
        --no-fork --config=$HOME/local/etc/x0/x0d.conf \
        --log-level=warn --log-file=$HOME/local/var/log/x0/x0d.log

