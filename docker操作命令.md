# Docker常用命令

## 1、docker system df 

![image-20211203193630652](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203193630652.png)

## 2、df -h

![image-20211203193728511](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203193728511.png)

## 3、fdisk -l

![image-20211203193811190](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203193811190.png)

## 4、查询容器log

```tex
ls -alh $(docker inspect $i | grep LogPath|awk -F "\"" '{print $4}')
```

![image-20211203194902920](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203194902920.png)

## 5、清理无用的容器相关数据（镜像、容器、volumes等）

```tex
docker system prune -a
```

## 6、查看目录下的文件大小

```tex
du -sh *
```

![image-20211203200029058](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203200029058.png)

## 7、根据容器目录名称（如上图目录），查看具体容器

```tex
docker ps -q | xargs docker inspect --format '{{.State.Pid}}, {{.Name}}, {{.GraphDriver.Data.WorkDir}}' | grep "f13afff4b8f29aa7fc10d161e564c516da357c2af87ec206e2dc7ed1510eaad2"
```

![image-20211203200136807](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211203200136807.png)















