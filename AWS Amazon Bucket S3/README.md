# Amazon Bucket S3 AWS
默认的Amazon Bucket名字是
```
http://s3.amazonaws.com/[bucket_name]/
http://[bucket_name].s3.amazonaws.com/
```

将一个文件移动到bucket中
```
sudo apt install awscli

touch test.txt
aws s3 mv test.txt s3://hackerone.marketing
FAIL : "move failed: ./test.txt to s3://hackerone.marketing/test.txt A client error (AccessDenied) occurred when calling the PutObject operation: Access Denied."

aws s3 mv test.txt s3://hackerone.files
SUCCESS : "move: ./test.txt to s3://hackerone.files/test.txt"
```

Bucket Finder
一个很棒的工具，可以用来查找可读的bucket，并且列出这些bucket里面的所有文件。它也可以用来找到那些存在的，但是拒绝列出文件请求的bucket。
```
wget https://digi.ninja/files/bucket_finder_1.1.tar.bz2 -O bucket_finder_1.1.tar.bz2
./bucket_finder.rb my_words
./bucket_finder.rb --region ie my_words
	US Standard         = http://s3.amazonaws.com
	Ireland             = http://s3-eu-west-1.amazonaws.com
	Northern California = http://s3-us-west-1.amazonaws.com
	Singapore           = http://s3-ap-southeast-1.amazonaws.com
	Tokyo               = http://s3-ap-northeast-1.amazonaws.com

./bucket_finder.rb --download --region ie my_words
./bucket_finder.rb --log-file bucket.out my_words
```
对bucket finder使用自定义列表，可以通过以下方式创建：
```
List of Fortune1000 company names with permutations on .com, -backup, -media. For example, walmart becomes walmart, walmart.com, walmart-backup, walmart-media.
List of the top Alexa 100,000 sites with permutations on the TLD and www. For example, walmart.com becomes www.walmart.com, www.walmart.net, walmart.com, and walmart.
```


## 致谢
* https://community.rapid7.com/community/infosec/blog/2013/03/27/1951-open-s3-buckets
* https://digi.ninja/projects/bucket_finder.php
