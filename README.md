# Nextcloud with Facerecognition

Basically we are pulling __nextcloud:apache__ image and installing dependencies __dlib__ and __pdlib__. Also, setting up a __cron__ container to run __background_job__ every minute. But __Facereconition__ is not installed by default, need to install from __Nextcloud__ web interface.  
It is possible to install __Facereconition__ directly from Dockerfile: clone the repo and compile. But this step needs to install __nodejs__, __npm__ which takes a long time and compile __Facereconition__ also takes a long time.  

It will create a volume in current folder. **_Change it_** before running docker compose.

### After install __Facereconition__  
After running this docker-compose and installing __Facereconition__ from __Nextcloud__, need to run these commands in the host system.  

```console
docker exec --user www-data <CONTAINER_ID> /var/www/html/occ face:setup -M 2048M
docker exec --user www-data <CONTAINER_ID> /var/www/html/occ face:setup --model 1
docker exec --user www-data <CONTAINER_ID> /var/www/html/occ config:app:set facerecognition min_image_size --value 10
```

And after that, go to Configurations.  
- In Personal, select Facerecognition and allow your images to be analyzed
- In Admin, select Facerecognition and set max image size

Start uploading images :)
