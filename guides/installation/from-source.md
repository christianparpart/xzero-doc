# Installing x0 From source

    git clone git://github.com/trapni/x0.git
    cd x0
    cmake -DCMAKE_BUILD_PREFIX=/opt/x0
    make && sudo make install

### First run!

    /opt/x0/bin/x0d --instant=`pwd`,8080
