FROM nextcloud:apache
RUN apt-get update \
 && apt-get install -y \
    cmake \
    git \
    libbz2-dev \
    libx11-dev \
 && apt-get clean

ARG DLIB_TAG=${DLIB_TAG:-v19.19}
RUN cd \
 && git clone https://github.com/davisking/dlib.git \
 && cd dlib \
 && git checkout $DLIB_TAG -b $DLIB_TAG \
 && cd dlib \
 && mkdir build \
 && cd build \
 && cmake -DBUILD_SHARED_LIBS=ON .. \
 && make -j10 \
 && make install

ARG PDLIB_BRANCH=${PDLIB_BRANCH:-master}
RUN cd \
 && git clone https://github.com/goodspb/pdlib.git \
 && cd pdlib \
 && git checkout $PDLIB_BRANCH \
 && phpize \
 && cat configure | sed 's/std=c++11/std=c++14/g' > configure_new \
 && chmod +x configure_new \
 && ./configure_new \
 && make -j10 \
 && make install
RUN docker-php-ext-enable pdlib

RUN docker-php-ext-install bz2

ENV PHP_MEMORY_LIMIT 2048M

RUN echo '* * * * * php /var/www/html/occ face:background_job -t 900 >>/var/www/html/data/nextcloud.log 2>>/var/www/html/data/nextcloud.log' >> /var/spool/cron/crontabs/www-data

# after here nextcloud is installed with php dependencies, still need to go into Apps and install Facerecognition
# if installing from Nextcloud, it will be in /var/www/html/custom_apps
# after installing Facerecognition, run these commands outside container to configure Face
#sudo docker exec --user www-data <CONTAINER_ID> /var/www/html/occ face:setup -M 2048M
#sudo docker exec --user www-data <CONTAINER_ID> /var/www/html/occ face:setup --model 1
#sudo docker exec --user www-data <CONTAINER_ID> /var/www/html/occ config:app:set facerecognition min_image_size --value 10

# and after that, go to Configurations.
# In Personal, select Facerecognition and allow your images to be analyzed
# In Admin, select Facerecognition and set max image size

