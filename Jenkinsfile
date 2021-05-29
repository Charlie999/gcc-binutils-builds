pipeline {
  agent any
  stages {
    stage('Download Binutils') {
      steps {
        sh 'wget $(cat binutils.url) -O binutils.tar.gz'
        sh 'tar xzf binutils.tar.gz'
        sh 'rm binutils.tar.gz'
        sh 'mv binutils-* binutils-src'
      }
    }

    stage('Build i686-elf binutils') {
      steps {        
        sh 'mkdir i686-binutils-build'
        sh 'mkdir i686-toolchain-lin'
        sh 'cd i686-binutils-build && ../binutils-src/configure --target=i686-elf --prefix="$(pwd)/../i686-toolchain-lin/" --with-sysroot --disable-nls --disable-werror'
        sh 'cd i686-binutils-build && make -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd i686-binutils-build && make install'
      }
    }
    
    stage('Build x86_64-elf binutils') {
      steps {
        sh 'mkdir x86_64-binutils-build'
        sh 'mkdir x86_64-toolchain-lin'
        sh 'cd x86_64-binutils-build && ../binutils-src/configure --target=x86_64-elf --prefix="$(pwd)/../x86_64-toolchain-lin/" --with-sysroot --disable-nls --disable-werror'
        sh 'cd x86_64-binutils-build && make -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd x86_64-binutils-build && make install'
      }
    }
    
    stage('Download GCC') {
      steps {
        sh 'wget $(cat gcc.url) -O gcc.tar.gz'
        sh 'tar xzf gcc.tar.gz'
        sh 'rm gcc.tar.gz'
        sh 'mv gcc-* gcc-src'
      }
    }
    
    stage('Build i686-elf gcc') {
      steps {        
        sh 'mkdir i686-gcc-build'
        sh 'cd i686-gcc-build && ../gcc-src/configure --target=i686-elf --prefix="$(pwd)/../i686-toolchain-lin/" --disable-nls --enable-languages=c,c++ --without-headers'
        sh 'cd i686-gcc-build && make all-gcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd i686-gcc-build && make all-target-libgcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd i686-gcc-build && make install-gcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd i686-gcc-build && make install-target-libgcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'tar czf i686-elf-linux-toolchain-gcc+binutils.tar.gz i686-toolchain-lin/'
      }
    }
    
    stage('Build x86_64-elf gcc') {
      steps {        
        sh 'mkdir x86_64-gcc-build'
        sh 'cd x86_64-gcc-build && ../gcc-src/configure --target=x86_64-elf --prefix="$(pwd)/../x86_64-toolchain-lin/" --disable-nls --enable-languages=c,c++ --without-headers'
        sh 'cd x86_64-gcc-build && make all-gcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd x86_64-gcc-build && make all-target-libgcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd x86_64-gcc-build && make install-gcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'cd x86_64-gcc-build && make install-target-libgcc -j$(grep -c processor /proc/cpuinfo)'
        sh 'tar czf x86_64-elf-linux-toolchain-gcc+binutils.tar.gz x86_64-toolchain-lin/'
      }
    }
  }
  post {
      always {
          archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
      }
  }
}
