# docker build -t yandex/clickhouse-stateless-with-coverage-test .
# TODO: that can be based on yandex/clickhouse-stateless-test (llvm version and CMD differs)
FROM yandex/clickhouse-deb-builder

ARG odbc_driver_url="https://github.com/ClickHouse/clickhouse-odbc/releases/download/v1.1.4.20200302/clickhouse-odbc-1.1.4-Linux.tar.gz"

RUN echo "deb [trusted=yes] http://apt.llvm.org/bionic/ llvm-toolchain-bionic-9 main" >> /etc/apt/sources.list

RUN apt-get update -y \
    && env DEBIAN_FRONTEND=noninteractive \
        apt-get install --yes --no-install-recommends \
            bash \
            tzdata \
            fakeroot \
            debhelper \
            zookeeper \
            zookeeperd \
            expect \
            python \
            python-lxml \
            python-termcolor \
            python-requests \
            curl \
            sudo \
            openssl \
            ncdu \
            netcat-openbsd \
            telnet \
            tree \
            moreutils \
            brotli \
            gdb \
            lsof \
            llvm-9 \
            unixodbc \
            wget \
            qemu-user-static

RUN mkdir -p /tmp/clickhouse-odbc-tmp \
   && wget --quiet -O - ${odbc_driver_url} | tar --strip-components=1 -xz -C /tmp/clickhouse-odbc-tmp \
   && cp /tmp/clickhouse-odbc-tmp/lib64/*.so /usr/local/lib/ \
   && odbcinst -i -d -f /tmp/clickhouse-odbc-tmp/share/doc/clickhouse-odbc/config/odbcinst.ini.sample \
   && odbcinst -i -s -l -f /tmp/clickhouse-odbc-tmp/share/doc/clickhouse-odbc/config/odbc.ini.sample \
   && rm -rf /tmp/clickhouse-odbc-tmp

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
COPY run.sh /run.sh

CMD ["/bin/bash", "/run.sh"]
