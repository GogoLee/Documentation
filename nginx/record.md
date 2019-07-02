# nginx访问量统计
## 1.根据访问IP统计UV
 awk '{print $1}' access.log|sort | uniq -c |wc -l
## 2.统计访问URL统计PV
awk '{print $7}' access.log|wc -l
## 3.查询访问最频繁的URL
awk '{print $7}' access.log|sort | uniq -c |sort -n -k 1 -r|more
## 4.查询访问最频繁的IP
awk '{print $1}' access.log|sort | uniq -c |sort -n -k 1 -r|more
## 5.根据时间段统计查看日志
cat access.log| sed -n '/14\/Mar\/2015:21/,/14\/Mar\/2015:22/p'|more
